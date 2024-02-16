======================
Export and import data
======================

In Odoo, it is sometimes necessary to export or import data for running reports or for data
modification. This document will cover the export and import of data into and out of Odoo.

.. important::
   Sometimes, users run into a 'time out' error, or a record will not process due to its size. This
   can occur with large exports or in cases where the import file is too large. To circumvent this
   limitation surrounding the size of the records, process exports or imports in smaller batches.

.. _export-data:

Export data from Odoo
=====================

When working with a database, it sometimes is necessary to export data in a distinct file. Doing so
can aid in reporting on activities (although Odoo provides a precise and easy reporting tool with
each available application).

With Odoo, the values can be exported from any field in any record. To do so, activate the list view
on the items that need to be exported and then select the records that should be exported. Finally,
click on :guilabel:`Action`, and then :guilabel:`Export`.

.. image:: export_import_data/list-view-export.png
   :align: center
   :alt: View of the different things to enable/click to export data.

When clicking on :guilabel:`Export`, a pop-up window appears with several options for the data to
export:

.. image:: export_import_data/export-data-overview.png
   :align: center
   :alt: Overview of options to consider when exporting data in Odoo..

#. With the :guilabel:`I want to update data (import-compatable export)` option ticked, the system
   only shows the fields that can be imported. This is helpful in the case where the existing
   records need to be updated. This works like a filter. Leaving the box unchecked gives many more
   field options because it shows all the fields, not only just the ones that can be imported.
#. When exporting, there is the option to export in two formats: `.csv` and `.xls`. With `.csv`,
   items are separated by a comma, while .xls holds information about all the worksheets in a file,
   including both content and formatting.
#. These are the items that can be exported. Use the :guilabel:`> (arrow)` to display more sub-field
   options. Use the :guilabel:`Search` bar to find specific fields more easily. To use the
   :guilabel:`Search` option more efficiently, click on all the :guilabel:`> (arrows)` to display
   all fields.
#. The :guilabel:`+ (plus)` button is present to add fields to the :guilabel:`Fields to export`
   list.
#. The :guilabel:`🕂 (cross)` next to the selected fields allows the fields to move up and down to
   change the order in which they must be displayed in the exported file. Drag-and-drop from the
   :guilabel:`🕂 (cross)`.
#. #. The :guilabel:`🗑️ (trash can)` is used to remove fields. Click on the :guilabel:`🗑️
   (trashcan)` to remove the field.
#. For recurring reports, it is helpful to save export presets. Select all the needed fields and
   click on the template drop-down menu. Once there, click on :guilabel:`New template` and give a
   unique name to the export just created. The next time the same list needs to be exported, select
   the related template that was previously saved in the drop-down menu.

.. tip::
   It is helpful to know the field's external identifier. For example, :guilabel:`Related Company`
   in the export user interface is equal to *parent_id* (external identifier). Doing so helps so
   that the only data exported is what should be modified and re-imported.

.. _import-data:

Import data into Odoo
=====================

Importing data into Odoo is extremely helpful during implementation or in times where data needs to
be updated in bulk. The following documentation covers importing data into an Odoo database.

.. warning::
   How to delete or undo an import of data into the Odoo database? Imports are permanent and Cannot
   be undone. However, it is possible to use filters (`on created on` or `last modified`) to
   identify records changed or created by the import.

.. tip::
   Activating :ref:`developer mode <developer-mode>` will change the visible import settings in the
   left menu. Doing so reveals an :menuselection:`Advanced` menu. Included in this advanced menu are
   two new options: :guilabel:`Track history during import` and :guilabel:`Allow matching with
   subfields`.

   .. image:: export_import_data/advanced-import.png
      :align: center
      :alt: Advanced import options when developer mode is activated.

   If the model uses openchatter, the :guilabel:`Track history during import` option will set up
   subscriptions and send notifications during the import, but lead to a slower import.

   Should the :guilabel:`Allow matching with subfields` option be selected then all subfields within
   a field will be used to match under the :guilabel:`Odoo Field` while importing.

How to start
------------

Data can be imported on any Odoo's business object using either Excel (`.xlsx`) or CSV (`.csv`)
formats. This includes: contacts, products, bank statements, journal entries, and even orders!

Open the view of the object to which the data should be imported/populated and click on
:menuselection:`Favorites --> Import records`.

.. image:: export_import_data/import_button.png
   :align: center
   :alt:  Favorites menu revealed with the import records option highlighted.

After clicking :guilabel:`Import records`, the page redirects to a page with templates that can be
downloaded and populated with the company's own data. Such templates can be imported in one click as
the data mapping is already done.

.. important::
   When importing a :abbr:`CSV (Comma-separated Values)` file Odoo will provide *Formatting*
   options. These *Formatting* options will not appear when importing the proprietary Excel file
   type (.xls, .xlsx).

   .. image:: export_import_data/formatting.png
      :align: center
      :alt: Formatting options presented when a CVS file is imported in Odoo.

How to adapt template
---------------------

- Add, remove, and sort columns to best fit the data structure.
- It is strongly advised to **not** remove the :guilabel:`External ID` (ID) column (see why in the
  next section).
- Set a unique ID to every record by dragging down the ID sequencing.

.. image:: export_import_data/dragdown.gif
   :align: center
   :alt: An animation of the mouse dragging down the ID column so that each record has a unique ID.

* When a new column is added, Odoo may not be able to map it automatically if its label does not fit
  any field within Odoo. Don't worry! New columns can be mapped manually when the import is tested.
  Search the drop-down menu for the corresponding field.

    .. image:: export_import_data/field_list.png
       :align: center
       :alt: Drop-down menu expanded in the initial import screen on Odoo.

  Then, use this field's label in the import file to ensure a successful import right away the next
  time.

  .. tip::
     Another useful way to find out the proper column names to import without issues is to export a
     sample file using the fields that should be imported. This way, if there is not a sample import
     template the names are accurate.

How to import from another application
--------------------------------------

To re-create relationships between different records, the unique identifier from the original
application should be used to map it to the :guilabel:`External ID` (ID) column in Odoo.

When another record is imported that links to the first one use **XXX/ID** (XXX/External ID) to the
original unique identifier. This record can also be found using its name. It should be noted that
there will be a conflict if at least two records have the same name.

The :guilabel:`External ID` (ID) will also be used to update the original import if modified data
needs to be re-imported later, therefore, it is a good practice to specify it whenever possible.

Cannot find the field to map column to
--------------------------------------

Odoo heuristically tries to find, based on the first ten lines of the files, the type of field for
each column inside the imported file. For example, if there is a column only containing numbers,
only the fields that are of type *integer* will be displayed to choose from.

While this behavior might be good and easy in most cases, it is also possible that it goes wrong or
that the column should be mapped to a field that is not proposed by default.

If this happens, check the :guilabel:`Show fields of relation fields (advanced) option`, then a
complete list of fields will be available for each column.

.. image:: export_import_data/field_list.png
   :align: center
   :alt: Searching for the field to match the tax column.

Where can date import format be changed?
----------------------------------------

.. note::
   Odoo can automatically detect if a column is a date, and it will try to guess the date format
   from a set of most commonly used date formats. While this process can work for many date formats,
   some date formats will not be recognized. This can cause confusion due to day-month inversions;
   it is difficult to guess which part of a date format is the day and which part is the month in a
   date such as '01-03-2016'.

When importing a :abbr:`CSV (Comma-separated Values)` file Odoo will provide *Formatting* options.
To view which date format Odoo has found from the file, check the :guilabel:`Date Format` that is
shown when clicking on options under the file selector. If this format is incorrect, change it to
the preferred format using *ISO 8601* to define the format.

.. tip::
   ISO 8601 is an international standard covering the worldwide exchange and communication of date
   and time-related data.

.. important::
   If an Excel (.xls, .xlsx) file is being imported, use date cells to store dates. The display of
   dates in Excel is different from the way they are stored. Storing dates in date cells will ensure
   that the date format is correct in Odoo, whatever locale the date format is in.

Can numbers with currency sign be imported (e.g.: $32.00)?
----------------------------------------------------------

Yes, Odoo fully supports numbers with parenthesis to represent negative sign as well as numbers with
currency sign attached to them. Odoo also automatically detects which thousand/decimal separator is
used. If a currency symbol unknown to Odoo is used, it might not be recognized as a number, and the
import will crash.

.. note::
   When importing a :abbr:`CSV (Comma-separated Values)` file, the formatting menu will appear on
   the left-hand column. Under these options, the :guilabel:`Thousands Separator` can be changed.

Examples of supported numbers (using thirty-two thousands as an example):

- 32.000,00
- 32000,00
- 32,000.00
- -32000.00
- (32000.00)
- $ 32.000,00
- (32000.00 €)

Example that will not work:

- ABC 32.000,00
- $ (32.000,00)

.. important::
   A :guilabel:`() (parenthesis)` around the number indicates that the number is a negative value.
   The currency symbol **must** be placed within the parenthesis for Odoo to recognize it as a
   negative currency value.

What can be done when Import preview table is not displayed correctly?
----------------------------------------------------------------------

By default, the import preview is set on commas as field separators and quotation marks as text
delimiters. If the :abbr:`CSV (Comma-separated Values)` file does not have these settings, modify
the :guilabel:`Formatting` options (displayed under the Browse CSV file bar after selecting the
:abbr:`CSV (Comma-separated Values)` file).

.. important::
   If the :abbr:`CSV (Comma-separated Values)` file has a tabulation as a separator, Odoo will
   **not** detect the separations. The file format options will need to be modified in the
   spreadsheet application. See the following question.

How to change to CSV file format options when saving in spreadsheet application
-------------------------------------------------------------------------------

When editing and saving :abbr:`CSV (Comma-separated Values)` files in spreadsheet applications, the
computer's regional settings will be applied for the separator and delimiter. Odoo suggests using
OpenOffice or LibreOffice Calc as both applications will allow modification of all three options
(from the application, go to :menuselection:`'Save As' dialog box --> Check the box 'Edit filter
settings' --> Save`).

Microsoft Excel will modification of the encoding when saving (in :menuselection:`'Save As' dialog
box --> 'Tools' drop-down menu --> Encoding tab`).

What's the difference between Database ID and External ID?
----------------------------------------------------------

Some fields define a relationship with another object. For example, the country of a contact is a
link to a record of the 'Country' object. When such fields are imported, Odoo will have to recreate
links between the different records. To help import such fields, Odoo provides three mechanisms.
**Only one** mechanism should be used per field that is imported.

For example, to reference the country of a contact, Odoo proposes three different fields to import:

- Country: the name or code of the country
- Country/Database ID: the unique Odoo ID for a record, defined by the ID postgresql column
- Country/External ID: the ID of this record referenced in another application (or the .XML file
  that imported it)

For the country Belgium, use one of these three ways to import:

- Country: Belgium
- Country/Database ID: 21
- Country/External ID: base.be

According to the company's need, use one of these three ways to reference records in relations. Here
is an example when one or the other should be used, according to the need:

- Use Country: this is the easiest way when data comes from :abbr:`CSV (Comma-separated Values)`
  files that have been created manually.
- Use Country/Database ID: this should rarely be used. It is mostly used by developers as the main
  advantage is to never have conflicts (there may be several records with the same name, but they
  always have a unique Database ID)
- Use Country/External ID: use External ID when importing data from a third-party application.

When External IDs are used, import :abbr:`CSV (Comma-separated Values)` files with the
:guilabel:`External ID` (ID) column defining the External ID of each record that is imported. Then,
a reference can be made to that record with columns like `Field/External ID`. The following two CSV
files provide an example for products and their categories.

- :download:`CSV file for categories
  <export_import_data/External_id_3rd_party_application_product_categories.csv>`

- :download:`CSV file for Products
  <export_import_data/External_id_3rd_party_application_products.csv>`

What can be done if there are multiple matches for a field?
-----------------------------------------------------------

If, for example, there are two product categories with the child name `Sellable` (e.g. `Misc.
Products/Sellable` & `Other Products/Sellable`), the validation is halted, but the data may still be
imported. However, Odoo recommends that the data is not imported because it will all be linked to
the first `Sellable` category found in the Product Category list (`Misc. Products/Sellable`). Odoo,
instead recommends modifying one of the duplicates' values or the product category hierarchy.

However, if the company does not wish to change the configuration of product categories, Odoo
recommends making use of the external ID for this field, 'Category'.

How to import a many2many relationship field (e.g. a customer that has multiple tags)?
--------------------------------------------------------------------------------------

The tags should be separated by a comma without any spacing. For example, a customer needs to be
linked to both tags: `Manufacturer` and `Retailer` then "Manufacturer,Retailer" will need to be
encoded in the same column of the :abbr:`CSV (Comma-separated Values)` file.

- :download:`CSV file for Manufacturer, Retailer <export_import_data/m2m_customers_tags.csv>`


How to import a one2many relationship (e.g. several Order Lines of a Sales Order)?
----------------------------------------------------------------------------------

If a company wants to import a sales order with several order lines, a specific row must be reserved
in the :abbr:`CSV (Comma-separated Values)` file for each order line. The first order line will be
imported on the same row as the information relative to order. Any additional lines will need an
additional row that does not have any information in the fields relative to the order.

As an example, here is :abbr:`CSV (Comma-separated Values)` file of some quotations that can be
imported, based on demo data:

- :download:`File for some Quotations
  <export_import_data/purchase.order_functional_error_line_cant_adpat.csv>`.

The following :abbr:`CSV (Comma-separated Values)` file shows how to import purchase orders with
their respective purchase order lines:

- :download:`Purchase orders with their respective purchase order lines
  <export_import_data/o2m_purchase_order_lines.csv>`.

The following :abbr:`CSV (Comma-separated Values)` file shows how to import customers and their
respective contacts:

- :download:`Customers and their respective contacts
  <export_import_data/o2m_customers_contacts.csv>`.

Can the same record be imported several times?
----------------------------------------------

If a file is imported that contains one of the columns: `External ID` or `Database ID`, records that
have already been imported will be modified instead of being created. This is extremely useful as it
allows to import the same :abbr:`CSV (Comma-separated Values)` file several times while having made
some changes in between two imports. Odoo will take care of creating or modifying each record
depending if it is new or not.

This feature allows a company to use the Import/Export tool in Odoo to modify a batch of records in
a spreadsheet application.

What happens if a value is not provided for a specific field?
-------------------------------------------------------------

If all fields are not set in the CSV file, Odoo will assign the default value for every non-defined
fields. But if fields are set with empty values in the :abbr:`CSV (Comma-separated Values)` file,
Odoo will set the EMPTY value in the field, instead of assigning the default value.

How to export/import different tables from an SQL application to Odoo?
----------------------------------------------------------------------

If data needs to be imported from different tables, relations will need to be recreated between
records belonging to different tables. (e.g. if companies and persons are imported, the link between
each person and the company they work for will need to be recreated).

To manage relations between tables, use the `External ID` facilities of Odoo. The `External ID` of a
record is the unique identifier of this record in another application. The `External ID` must be
unique across all records of all objects. It is a good practice to prefix this `External ID` with
the name of the application or table. (like 'company_1', 'person_1' instead of '1')

As an example, suppose there is a SQL database with two tables that are to be imported: companies
and persons. Each person belongs to one company, so the link between a person and the company they
work for need to be recreated.

Test this example, here is a :download:`dump of such a PostgreSQL database
<export_import_data/database_import_test.sql>`

First, export all companies and their "External ID". In PSQL, write the following command:

.. code-block:: sh

   > copy (select 'company_'||id as "External ID",company_name as "Name",'True' as "Is a Company" from companies) TO '/tmp/company.csv' with CSV HEADER;

This SQL command will create the following :abbr:`CSV (Comma-separated Values)` file:

.. code-block:: text

   External ID,Name,Is a Company
   company_1,Bigees,True
   company_2,Organi,True
   company_3,Boum,True

To create the :abbr:`CSV (Comma-separated Values)` file for persons, linked to companies, use the
following SQL command in PSQL:

.. code-block:: sh

    > copy (select 'person_'||id as "External ID",person_name as "Name",'False' as "Is a Company",'company_'||company_id as "Related Company/External ID" from persons) TO '/tmp/person.csv' with CSV

It will produce the following :abbr:`CSV (Comma-separated Values)` file:

.. code-block:: text

   External ID,Name,Is a Company,Related Company/External ID
   person_1,Fabien,False,company_1
   person_2,Laurence,False,company_1
   person_3,Eric,False,company_2
   person_4,Ramsy,False,company_3

As can be seen in this file, Fabien and Laurence are working for the Bigees company (company_1) and
Eric is working for the Organi company. The relation between persons and companies is done using the
`External ID` of the companies. The `External ID` is prefixed by the name of the table to avoid a
conflict of ID between persons and companies (person_1 and company_1 who shared the same ID 1 in the
original database).

The two files produced are ready to be imported in Odoo without any modifications. After having
imported these two :abbr:`CSV (Comma-separated Values)` files, there are four contacts and three
companies (the first two contacts are linked to the first company). Keep in mind to first import
the companies and then the persons.

How to adapt an import template
===============================

Import templates are provided in the import tool of the most common data to import (contacts,
products, bank statements, etc.). Open them with any spreadsheet software (Microsoft Office,
OpenOffice, Google Drive, etc.).

How to customize file
=====================

* Remove columns that are not needed. Odoo advises not removing the :guilabel:`External ID` (ID)
  column (see why below).
* Set a unique ID to every single record by dragging down the ID sequencing.

  .. image:: export_import_data/dragdown.gif
     :align: center
     :alt: Animation of a spreadsheet application automatically populating a column in the ID.

* When adding a new column, Odoo might not map it automatically if its label does not match any
  field in the system. If so, find the corresponding field using the search.

    .. image:: export_import_data/field_list.png
       :align: center
       :alt: Searching for the field in the drop-down menu of the import page.

  Then, use the label found in the import template to make it work right away the next time the
  import is attempted.

Why an “ID” column
==================

The :guilabel:`ID` (External ID) is an unique identifier for the line item. Feel free to use one
from previous software to facilitate the transition to Odoo.

Setting an ID is not mandatory when importing but it helps in many cases:

* Update imports: import the same file several times without creating duplicates;
* Import relation fields (see below).

How to import relation fields
=============================

An Odoo object is always related to many other objects (e.g. a product is linked to product
categories, attributes, vendors, etc.). To import those relations, the records of the related object
need to be imported first, from their own list menu.

This can be achieved by using either the name of the related record or its ID, depending on the
circumstances. The ID is expected when two records have the same name. In such a case add ` / ID`
at the end of the column title (e.g. for product attributes: Product Attributes / Attribute / ID).
