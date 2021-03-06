################
HTML Table Class
################

The Table Class provides functions that enable you to auto-generate HTML
tables from arrays or database result sets.

Initializing the Class
======================

Like most other classes in CodeIgniter, the Table class is initialized
in your controller using the $this->load->library function::

	$this->load->library('table');

Once loaded, the Table library object will be available using:
$this->table

Examples
========

Here is an example showing how you can create a table from a
multi-dimensional array. Note that the first array index will become the
table heading (or you can set your own headings using the set_heading()
function described in the function reference below).

::

	$this->load->library('table');

	$data = array(
	             array('Name', 'Color', 'Size'),
	             array('Fred', 'Blue', 'Small'),
	             array('Mary', 'Red', 'Large'),
	             array('John', 'Green', 'Medium')	
	             );

	echo $this->table->generate($data);

Here is an example of a table created from a database query result. The
table class will automatically generate the headings based on the table
names (or you can set your own headings using the set_heading()
function described in the function reference below).

::

	$this->load->library('table');

	$query = $this->db->query("SELECT * FROM my_table");

	echo $this->table->generate($query);

Here is an example showing how you might create a table using discrete
parameters::

	$this->load->library('table');

	$this->table->set_heading('Name', 'Color', 'Size');

	$this->table->add_row('Fred', 'Blue', 'Small');
	$this->table->add_row('Mary', 'Red', 'Large');
	$this->table->add_row('John', 'Green', 'Medium');

	echo $this->table->generate();

Here is the same example, except instead of individual parameters,
arrays are used::

	$this->load->library('table');

	$this->table->set_heading(array('Name', 'Color', 'Size'));

	$this->table->add_row(array('Fred', 'Blue', 'Small'));
	$this->table->add_row(array('Mary', 'Red', 'Large'));
	$this->table->add_row(array('John', 'Green', 'Medium'));

	echo $this->table->generate();

Changing the Look of Your Table
===============================

The Table Class permits you to set a table template with which you can
specify the design of your layout. Here is the template prototype::

	$tmpl = array (
	                    'table_open'          => '<table border="0" cellpadding="4" cellspacing="0">',

	                    'heading_row_start'   => '<tr>',
	                    'heading_row_end'     => '</tr>',
	                    'heading_cell_start'  => '<th>',
	                    'heading_cell_end'    => '</th>',

	                    'row_start'           => '<tr>',
	                    'row_end'             => '</tr>',
	                    'cell_start'          => '<td>',
	                    'cell_end'            => '</td>',

	                    'row_alt_start'       => '<tr>',
	                    'row_alt_end'         => '</tr>',
	                    'cell_alt_start'      => '<td>',
	                    'cell_alt_end'        => '</td>',

	                    'table_close'         => '</table>'
	              );

	$this->table->set_template($tmpl);

.. note:: You'll notice there are two sets of "row" blocks in the
	template. These permit you to create alternating row colors or design
	elements that alternate with each iteration of the row data.

You are NOT required to submit a complete template. If you only need to
change parts of the layout you can simply submit those elements. In this
example, only the table opening tag is being changed::

	$tmpl = array ( 'table_open'  => '<table border="1" cellpadding="2" cellspacing="1" class="mytable">' );

	$this->table->set_template($tmpl);
	
You can also set defaults for these in a config file.

******************
Function Reference
******************

$this->table->generate()
========================

Returns a string containing the generated table. Accepts an optional
parameter which can be an array or a database result object.

$this->table->set_caption()
============================

Permits you to add a caption to the table.

::

	$this->table->set_caption('Colors');

$this->table->set_heading()
============================

Permits you to set the table heading. You can submit an array or
discrete params::

	$this->table->set_heading('Name', 'Color', 'Size');

::

	$this->table->set_heading(array('Name', 'Color', 'Size'));

$this->table->add_row()
========================

Permits you to add a row to your table. You can submit an array or
discrete params::

	$this->table->add_row('Blue', 'Red', 'Green');

::

	$this->table->add_row(array('Blue', 'Red', 'Green'));

If you would like to set an individual cell's tag attributes, you can
use an associative array for that cell. The associative key 'data'
defines the cell's data. Any other key => val pairs are added as
key='val' attributes to the tag::

	$cell = array('data' => 'Blue', 'class' => 'highlight', 'colspan' => 2);
	$this->table->add_row($cell, 'Red', 'Green');

	// generates
	// <td class='highlight' colspan='2'>Blue</td><td>Red</td><td>Green</td>

$this->table->make_columns()
=============================

This function takes a one-dimensional array as input and creates a
multi-dimensional array with a depth equal to the number of columns
desired. This allows a single array with many elements to be displayed
in a table that has a fixed column count. Consider this example::

	$list = array('one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve');

	$new_list = $this->table->make_columns($list, 3);

	$this->table->generate($new_list);

	// Generates a table with this prototype

	<table border="0" cellpadding="4" cellspacing="0">
	<tr>
	<td>one</td><td>two</td><td>three</td>
	</tr><tr>
	<td>four</td><td>five</td><td>six</td>
	</tr><tr>
	<td>seven</td><td>eight</td><td>nine</td>
	</tr><tr>
	<td>ten</td><td>eleven</td><td>twelve</td></tr>
	</table>

$this->table->set_template()
=============================

Permits you to set your template. You can submit a full or partial
template.

::

	$tmpl = array ( 'table_open'  => '<table border="1" cellpadding="2" cellspacing="1" class="mytable">' );

	$this->table->set_template($tmpl);

$this->table->set_empty()
==========================

Let's you set a default value for use in any table cells that are empty.
You might, for example, set a non-breaking space::

	 $this->table->set_empty("&nbsp;");

$this->table->clear()
=====================

Lets you clear the table heading and row data. If you need to show
multiple tables with different data you should to call this function
after each table has been generated to empty the previous table
information. Example::

	$this->load->library('table');

	$this->table->set_heading('Name', 'Color', 'Size');
	$this->table->add_row('Fred', 'Blue', 'Small');
	$this->table->add_row('Mary', 'Red', 'Large');
	$this->table->add_row('John', 'Green', 'Medium');

	echo $this->table->generate();

	$this->table->clear();

	$this->table->set_heading('Name', 'Day', 'Delivery');
	$this->table->add_row('Fred', 'Wednesday', 'Express');
	$this->table->add_row('Mary', 'Monday', 'Air');
	$this->table->add_row('John', 'Saturday', 'Overnight');

	echo $this->table->generate();

$this->table->function
======================

Allows you to specify a native PHP function or a valid function array
object to be applied to all cell data.

::

	$this->load->library('table');

	$this->table->set_heading('Name', 'Color', 'Size');
	$this->table->add_row('Fred', '<strong>Blue</strong>', 'Small');

	$this->table->function = 'htmlspecialchars';
	echo $this->table->generate();

In the above example, all cell data would be ran through PHP's
htmlspecialchars() function, resulting in::

	<td>Fred</td><td>&lt;strong&gt;Blue&lt;/strong&gt;</td><td>Small</td>

