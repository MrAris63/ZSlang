# ZSlang LANGUAGE

ZSlang is an easy, fast and simple language used to generate CRUD type systems for JavaScript, Node.js and Postgresql. It is born from the need to have an efficient way to quickly have queries, captures and information reports in production. It is divided into different sections to be interpreted and execute JavaScript functions both in the front (browser) and in the back (Node.js and Postgresql), since the code is generated to be executed in Node.js as well as creates or updates the database, both the definition of the tables and the stored procedures necessary to manage them.

## SECTIONS

ZSlang is divided into sections (yes, I know, old-fashioned Cobol style) that allow us to neatly define each of the parts of the system.
Sections must be preceded by an asterisk (as shown) and are as follows:

* **\*System**. The default functions to be used are specified
* **\*Name**. is the name of the system
* **\*Menu**. The menu structure to be used is specified
* **\*Database**. Specifies the name and structure of the database
* **\*Grid**. A data grid to be used is specified
* **\*Form**. A form of capture to be used is specified
* **\*Report**. Specifies a report to use

### *SYSTEM

This section specifies the JavaScript functions that will be used for the different functions of the System. The following functions are provided. The defaults are already programmed in the ZSlang example:

* **Headers**. Function to display the Header. Default: **displayHeader**
* **Menu**. Function to display the Menu. Default: **displayMenu**
* **Grid**. Function to display the Grids. Default: **displayGrid**
* **Form**. Function to display the Forms. Default: **displayForm**
* **Report**. Function to display the Report. Default: **displayReport**

It should be mentioned that each of the functions must take charge of the complete handling of each part, that is, **Menu** must take full charge of how to display the menu. By definition, there are already functions of each one of them. An example of code from this section called *System* is as follows:

     //ZSlang CRUD language example
     //This is a comment
     *SYSTEM
     Header displayHeader //Function to display the Header
     Menu displayMenu //Function to display the Menu
     Grid displayGrid //Function to display the Grid
     Form displayForm //Function to display the Form
     Report displayReport //Function to display the Report

The following specifies what each of these parts means within the System to be built:

<figure>
     <img src="images/Header.png" alt="System Header" title="System Header" width="500px">
     <figcaption>This header features a logo and the name of the system. Both are defined in the <em>*Name</em></figcaption> section.
</figure>

<figure>
     <img src="images/Menu.png" alt="System Menu" title="System Menu" width="500px">
     <figcaption>The <em>menu in this particular case</em> (and as it is in the code by definition) is on the side and is hidden or shown with the icon on the top left. You can change the functionality by creating a function that displays (and manages) the menu in the way that best suits you, you just have to put the following in the *SYSTEM section: <em>Menu myMenu</em> and already in that function called (in this case) "myMenu" you can make the menu that you see fit</figcaption>
</figure>

<figure>
     <img src="images/Grid.png" alt="Grid" title="Grid" width="500px">
     <figcaption>The grid <em>in this particular case</em> (and as it is in the code that it has by definition) is as shown in the image. You can change the functionality by creating a function that displays (and manages) the grids in the way that best suits you, you just have to put the following in the *SYSTEM section: <em>Grid myGrid</em> and already in that function called (in this case) "miGrid" you can make the grid that you see fit</figcaption>
</figure>

<figure>
     <img src="images/Form.png" alt="Form" title="Auto Form" width="500px">
     <figcaption>The form <em>in this particular case</em> (and as it is in the code that it has by definition) is as shown in the image. You can change the functionality by creating a function that displays (and manages) the forms in the way that best suits you, you just have to put the following in the *SYSTEM section: <em>Form myForm</em> and already in that function called (in this case) "myShape" you can make the shape that you see fit. In this case, the form automatically generated by ZSlang with the JavaScript functions that are by definition is shown. When defining your grid in the <em>+FORM</em> part, specify <em>AUTO</em> and that's it. That is, it would look like this: <em>+FORM AUTO</em></figcaption>
</figure>

Internally in the *js/ZSlang.js* file, the internal variables are defined where these data are stored:

     var _ZSlangDisplayHeader = null; // Function to display the header. Default: displayHeader() using the global var header
     var _ZSlangDisplayMenu = null; //Function to display the menu. Default: displayMenu() using the global var menus
     var _ZSlangDisplayGrid = null; //Function to display the grid. Default: displayGrid() using the global var grids
     var _ZSlangDisplayForm = null; //Function to display the from. Default: displayForm() using the global var forms
     var _ZSlangDisplayReport = null; //Function to display the report. Default: displayReport() using the globar var reports

### *NAME

This section helps us to define the name of the system and its identification icon (usually drop-downs in the Header). First the name of the system is indicated and then between braces **{ }** the image (or logo) of the System is specified. Example:

     *YAM
     ZSlang Language example {img\logo.png} //From here is a comment

You have a variable in *js/ZSlang.js* defined like this:

     var header = { //Header definition
     title: "Zafirosoft System",
     logo: "img/logo.png",
     };

In this structure, both the name of the System and its logo are defined.

## *MENU

As the name implies, the menu structure is defined here. The way to define the menus is very simple, since first the highest level option (also called level 0 option) is entered and then in another line the submenus of that level 0 option are defined using the *>* symbol. Example:

     *MENU
     System Catalogs {fa fa-pencil} //What is between {} is the icon
     >Item Groups {fa fa-heart} GRID:Group List//The last parameter is the link to call
     >Subgroups of Articles {fa fa-mobile} GRID: List of Subgroups
     >Items {fa fa-coffee} GRID:Items
     System Processes {fa fa-microchip}
     >Inventory Moves {fa fa-money} GRID:Grid Moves
     >Prices {fa fa-diamond} FORM:Prices
     Reports {fa fa-print}
     >Costing {fa fa-paw} REPORT:Item Costing Report
     >Obsolete {fa fa-map-signs} REPORT:Rep Obsolete
     {fa fa-truck} GRID Prop: Group List
     Test {fa fa-save} GRID:People

As can be seen, the definition of the level 0 menu is made up of:

* Description of the option
* Icon. This is defined between braces *{ }* and are the FontAwesome classes that define the icon to be displayed

On the other hand, the definition of the submenus is given **in the same line** each submenu and is composed of:

* \>Option description
* Icon. This one is defined between braces *{ }* and are the classes of FontAwesome
* url. Here you define the URL directly or the grid, form or report to call:
   * Grid: *Grid name*
   * Form: *Form name*
   * Report: *report name*

It is important to clarify that obviously the definition of each one (Grid, Form or Report) must be in the ZSlang source code.

Once the ZSlang source code is parsed, the menu definition is stored in the variable *menus*, which is an array with the following structure:

* caption: Text to display in the menu option
* icon: FontAwesome class that contains the icon of the menu option
* url: It is the URL to which it will go when selecting the option, or the name of the Grid, the Form or the Report that will be loaded when selecting the option. If the option is a menu that has a submenu, then you have an empty string **""**
* urlType: It is the type of url that will be called. If the option is a menu that has a submenu, then you have an empty string **""**:
   * url
   *GRID
   * FORM
   * REPORT
* subMenus: This is an array where the submenu options that belong to option level 0 are defined. Its structure is the same with *caption*, *icon*, *url* and *urlType*.

### *DATABASE

In this section the name of the database is defined, as well as the names of its tables and the characteristics of the fields of each table. Here's an example:

     *DATABASE ZSlangtest

     People
     >Name [Person Name] varchar 100 REQ
     >LastName [] varchar 100 REQ
     >Mail [Email] varchar 100 REQ
     >Sex [] char 1 REQ DEFAULT "H"
     >Time [] timestamp
     >factor [] int
     >Salary [] money
     >Birth date

Along with the section, the name of the database is defined. In this case from the previous example, the database is called *ZSlangtest*. Each table is defined very simply, since you simply put its name and then in each following line and starting with the symbol *>* each field of the table is defined.

**IMPORTANT:** Each table is automatically added (if not explicitly specified in the table) a field called *ID* which serves as the identifier (primary key), since it is autonumber (identity) of type * bigint*.

The definition of each field is done one per line, always preceded by *>*, after the required form between *[ ]* the label to be used. If no label is defined between the *[ ]*, then the field name is used, but since *[ ]* are required, they must be written one after the other.

After declaring the name of the field and its label to use with it, the type of data that the field is is defined. The following types of data are available:

**Type of data**

| In ZSlang | In PostgreSQL | Notes |
|---------|---------------|-------|
| char | char | Specify the number of characters. Example: CHAR 2 |
| characters | characters | Specify the number of characters. Example: CHARACTER 5 |
| varchar | varchar | Specify the number of characters. Example: VARCHAR 11 |
| text<br>memo | text |
| email | varchar(150) |
| smallint<br>small | smallint | signed two-byte integer |
| bigint<br>big | bigint | signed eight-byte integer |
| int8 | int8 | signed eight-byte integer |
| int | int | signed four-byte integer |
| integer | integer | signed four-byte integer |
| int4 | int4 | signed four-byte integer |
| money | numeric(20,5) | for maximum precision
| float8 | float8 | double precision floating-point number (8 bytes) |
| double | double precision | double precision floating-point number (8 bytes) |
| actual | actual | single precision floating-point number (4 bytes) |
| float4<br>float | float4 | single precision floating-point number (4 bytes) |
| numeric | numeric | Exact numeric of selectable precision. There are 3 scenarios:<br>1. Without specifying precision. Example: NUMERIC<br>2. With only precision. Example: NUMERIC 8<br>3. With precision and scale. Example: NUMERIC 8.3 |
| decimal | decimal | Exact numeric of selectable precision. There are 3 scenarios:<br>1. Without specifying precision. Example: DECIMAL<br>2. With only precision. Example: DECIMAL 8<br>3. With precision and scale. Example: DECIMAL 8.3 |
| boolean | boolean | logical Boolean (true/false) |
| boole | boole | logical Boolean (true/false) |
| date | date | calendar date (year, month, day) |
| time | time | time of day (no time zone) |
| time | time with time zone | time of day, including time zone |
| timestamp<br>datetime | timestamp | date and time (no time zone) |
| timestampz<br>datetimez | timestamp with time zone | date and time, including time zone |
| bytea<br>photo<br>img<br>image<br>file | bytea | binary data (“byte array”) |
| lookup | bigint | ID to parent table record |
| color | int4 | Color number |
| phone | varchar(40) | Telephone number |
| url | varchar(255) | Internet address |


For further reference see: [PostgreSQL](https://www.postgresql.org/docs/current/datatype.html)

Once the data type and its dimensions or pointers (to another table-field in the lookup) have been specified, as many validations as are required are specified in random order.

**Validations**

| Key word | Validation | Example |
|---------------|------------|---------|
| MIN | Minimum value allowed.<br>*Minimum value must be specified below* | MIN 8 |
| MAX | Maximum value allowed.<br>*Maximum value must be specified below* | MAX "zzz" |
| UNIQUE | The values of that field are unique<br>*That is, they cannot be repeated* | UNIQUE |
| REQUIRED<br>REQ | The value of that field is required | QR |
| UPPER | The value is converted to uppercase<br>*Only for character fields like CHAR, CHARACTER, VARCHAR, TEXT*| UPPER |
| LOWER | The value is converted to lowercase<br>*Only for character fields like CHAR, CHARACTER, VARCHAR, TEXT*| LOWER |
| DEFAULT | The default value of that field<br>*The default value must be specified below* | DEFAULT "Company" |
| DISABLED | The field in the screenshot will be disabled | DISABLED |
| HIDDEN | The field will not be visible anywhere | HIDDEN |
| READONLY<br>RO | The field is visible but cannot be changed | READONLY |

### *GRID

This section defines the grids or lists of information within the system. This section is divided into several subsections. Example:

     *GRID
     List of Subgroups {fa fa-microchip} Subgroups
     +FILTERS
     >Group
     +COLUMNS
     >ID
     >GroupID
     >Subgroup
     +FORM Catalog of Subgroups //It is captured in the form "Catalog of Subgroups"

As can be seen in the example, there are ***HERE I STAYED***
* Grid. The name of the grid, its icon and the table or stored procedure to be used to display the data are defined.
* +Filters. OPTIONAL. Here the fields that will be used as filters that will be used to display the grid are defined.
* +Columns. These are the columns that the grid will display.
* +Form. It is the form that will be used to update the grid data. If the form is not defined, then the grid will only serve to display the data, but not to insert, update or delete it.

In the first line there is the first subsection where the name of the grid is specified, its specified icon

### *FORM

### *REPORT

## FILE STRUCTURE

You have the following file structure:

### Directories

These are the directories that are available and their contents:

* root
   * routes
   *js
     * raw
   *img
   * htm
   *css
   * doc
     * pictures

## GLOBAL VARIABLES

After parsing the ZSlang source code, the following array-type global variables are generated that contain the necessary information to create the JavaScript and SQL code to make everything work perfectly. These variables are the following:

* **tables**. Here is all the data in the table:
   * **table**. The name of the table in the database.
   * **fields**. It is an array that defines the fields that the table contains. Each element (field) has the following data:
     * **caption**. It is the level to be displayed by default in grids, forms and reports.
     * **fieldName**. Name of the field in the database.
     * **numBytes**. Number of bytes that this field has. It is used for fields of type CHAR and VARCHAR. For all other data types this property is filled automatically.
     * **unique**. Specifies if this field its value is unique and unrepeatable in the entire table.

---

## TYPE OF DATA

* **char** - Can be capture or for radio buttons or combo box
   * Properties:
     * *REQ* or *REQUIRED* - Required. Default: **No**
     * *DEFAULT* - Default value. Default: **""**
     * *DISABLED* - Whether or not it is disabled. Default: **No**
     * *HIDDEN* - Whether or not it is hidden. Default: **No**
* **varchar** - For string input (can be formatted)
* **text** - Multiline text

**smallint** - Formatted number
