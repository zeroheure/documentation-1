=====
Italy
=====

.. _italy/modules:

Configuration
=============

:ref:`Install <general/install>` the following modules to get all the features of the Italian
localization:

.. list-table::
   :header-rows: 1
   :stub-columns: 1

   * - Name
     - Technical name
     - Description
   * - Italy - Accounting
     - `l10n_it`
     - Default :ref:`fiscal localization package <fiscal_localizations/packages>`
   * - Italy - E-invoicing
     - `l10n_it_edi`
     - e-invoice implementation
   * - Italy - E-invoicing
     - `l10n_it_edi_withholding`
     - e-invoice withholding
   * - Italy - Accounting Reports
     - `l10n_it_reports`
     - Country-specific reports
   * - Italy - Stock DDT
     - `l10n_it_stock_ddt`
     - Transport documents - Documento di Trasporto (DDT)

.. image:: italy/italy-modules.png
   :align: center
   :alt: Italian localization modules

Company information
-------------------

Configuring the company's information ensures your Accounting database is properly set up. To add
information, go to :menuselection:`Settings --> General Settings`, and in the :guilabel:`Companies`
section, click :guilabel:`Update info`. From here, fill out the fields:

- :guilabel:`Address`: the address of the company;
- :guilabel:`VAT`: VAT of the company;
- :guilabel:`Codice Fiscale`: the fiscal code of the company;
- :guilabel:`Tax System`: the tax system under which the company falls;

.. image:: italy/italy-company.png
   :align: center
   :alt: Company information to provide

Taxes
-----

Many of the e-invoicing features are implemented using Odoo's tax system. As such, it is very
important that taxes are properly configured in order to generate invoices correctly and handle
other billing use cases.

The **Italian** localization contains predefined **examples** of taxes for various purposes.

Tax Exemption
~~~~~~~~~~~~~~

.. _italy/tax-exemption:

The use of sale taxes that amount to **zero percent** (0%) is required to keep track of
the exact :guilabel:`Tax Exemption Kind (Natura)` and :guilabel:`Law Reference` that justify the
exemption operated on an invoice line.

For example the tax for export in the EU to be used as reference (`0% EU`, invoice label `00eu`),
which can be found under :menuselection:`Accounting --> Configuration --> Taxes`.
Exports are exempt from VAT, and therefore they require the :guilabel:`Has exoneration of tax (Italy)`
option ticked, with both the :guilabel:`Exoneration` kind and :guilabel:`Law Reference` filled in.

.. image:: italy/italy-tax.png
   :align: center
   :alt: Tax Exemption Settings

.. note::
   If you need to use a different kind of :guilabel:`Exoneration`, click :menuselection:`Action -->
   Duplicate` within the tax menu to create a copy of an existing similar tax. Then, select another
   :guilabel:`Exoneration`, and :guilabel:`Save`. Repeat this process as many times as you need
   different kind of :guilabel:`Exoneration` taxes.

.. tip::
   **Rename** your taxes in the :guilabel:`Name` field according to their :guilabel:`Exoneration` to
   differentiate them easily.

Reverse charge
==============

Italian businesses selling goods and services are sometimes required *not* to charge the customer
for the VAT. The customers pay the VAT *themselves* to the :abbr:`AdE (Agenzia delle Entrate)` instead.
This mechanism is called **reverse charge**. It can be :guilabel:`Internal Reverse Charge` (domestic
business) or :guilabel:`External Reverse Charge` (intra-EU business).

Invoices
--------

**Reverse charged** customer invoices are **tax exempt** (0%) for the seller. In this section we
will help you configure your :guilabel:`Tax Exemption Reason` and :guilabel:`Law Reference`.

    !! TODO: xxxxxxxxxx

Vendor bills
------------

Italian companies buying goods or services from EU countries (or services from non-EU countries)
must send the information contained within the bill received to the :abbr:`AdE (Agenzia delle Entrate)`. This
allows you to complete tax-related information on your bill, and to send it. The seller must be set
as :guilabel:`Cedente/Prestatore`, and the buyer as :guilabel:`Cessionario/Committente`. Contained
within the **XML** document for the vendor bill, the vendor's credentials show as
:guilabel:`Cedente/Prestatore`, and your company's credentials as
:guilabel:`Cessionario/Committente`.

.. note::
   VAT integrations must be issued and sent to the tax agency for reverse charged bills.

When inputting taxes in a vendor bill, it is possible to select **reverse charge** taxes. These are
automatically activated in the Italian fiscal position. By going to :menuselection:`Accounting -->
Configuration --> Taxes`, the `10%` and `22%` :guilabel:`Goods` and :guilabel:`Services` tax scopes
are activated and preconfigured with the correct tax grids. These are set up automatically to ensure
the correct booking of accounting entries and display of the tax report.

.. _italy/grids:

Tax grids
---------

The Italian localization has a specific **tax grid** section for **reverse charge** taxes. These tax
grids are identifiable by the :ref:`VJ <italy/grids>` tag, and can be found under
:menuselection:`Accounting --> Reporting --> Audit Reports: Tax Report`.

.. image:: italy/italy-grids.png
   :align: center
   :alt: Italian reverse charge tax grids

.. _italy/e-invoicing:

E-invoicing
===========

The :abbr:`SdI (Sistema di Interscambio)` is the electronic invoicing system used in Italy. It
enables to send and receive electronic invoices to and from customers. The documents must be in an
**XML** :abbr:`EDI (Electronic Data Interchange)` format called **FatturaPA** and formally validated
by the system before being delivered.

To be able to receive invoices and notifications, the :abbr:`SdI (Sistema di Interscambio)` service
must be notified that the user's files are to be sent to Odoo and processed on their behalf. To do
so, you must set up Odoo's :guilabel:`Codice Destinatario` on the :abbr:`AdE (Agenzia Delle
Entrate)` portal.

#. Go to https://ivaservizi.agenziaentrate.gov.it/portale/ and authenticate;
#. Go to section :menuselection:`Fatture e Corrispettivi`;
#. Set the user as Legal Party for the VAT number you wish to configure the electronic address;
#. In :menuselection:`Servizi Disponibili --> Fatturazione Elettronica --> Registrazione
   dell’indirizzo telematico dove ricevere tutte le fatture elettroniche`, insert Odoo's
   :guilabel:`Codice Destinatario` `K95IV18`, and confirm.

EDI Mode and authorization
--------------------------

Since the files are transmitted through Odoo's server before being sent to the :abbr:`SdI (Sistema
di Interscambio)` or received by your database, you need to authorize Odoo to process your files
from your database. To do so, go to :menuselection:`Accounting --> Configuration --> Settings -->
Electronic Document Invoicing`.

There are **three** modes available:

- :guilabel:`Demo`
  This mode simulates an environment in which invoices are sent to the government. In this mode,
  invoices need to be *manually* downloaded as XML files and uploaded to the :abbr:`AdE (Agenzia
  delle Entrate)`'s website.
- :guilabel:`Test (experimental)`
  This mode sends invoices to a non-production (i.e., test) service made available by the :abbr:`AdE
  (Agenzia delle Entrate)`. Saving this change directs all companies on the database to use this
  configuration.
- :guilabel:`Official`
  This is a production mode that sends your invoices directly to the :abbr:`AdE (Agenzia delle
  Entrate)`.

Once a mode is selected, you need to accept the **terms and conditions** by ticking :guilabel:`Allow
Odoo to process invoices`, and then :guilabel:`Save`. You can now record your transactions in Odoo
Accounting.

.. warning::
   Selecting either :guilabel:`Test (experimental)` or :guilabel:`Official` is **irreversible**.
   Once in :guilabel:`Official` mode, it is not possible to select :guilabel:`Test (experimental)`
   or :guilabel:`Demo`, and same for :guilabel:`Test (experimental)`. We recommend creating a
   separate database for testing purposes only.

.. note::
   When in :guilabel:`Test (Experimental)` mode, all invoices sent *must* have a partner using one
   of the following fake :guilabel:`Codice Destinatario` given by the :abbr:`AdE (Agenzia Delle Entrate)`:
   `0803HR0` - `N8MIMM9` - `X9XX79Z`. Any real production :guilabel:`Codice Destinario` of your
   customers will not be recognized as valid by the test service.

.. image:: italy/italy-edi.png
   :align: center
   :alt: Italy's electronic document invoicing options

.. _italy/e-invoicing-process:

Process
-------

The submission of invoices to the :abbr:`SdI (Sistema di Interscambio)` for Italy is an electronic
process used for the mandatory transmission of tax documents in **XML** format between companies and
the :abbr:`AdE (Agenzia delle Entrate)` to reduce errors and verify the correctness of operations.

.. note::
   You can check the current status of an invoice by the :guilabel:`SdI State` field.
   The XML file can be found as an **attachment** of the invoice.

.. image:: italy/italy-edi-process.png
   :align: center
   :alt: EDI Process

1. XML Documents creation
~~~~~~~~~~~~~~~~~~~~~~~~~

Odoo generates the required `XML` files as attachments to invoices in the `FatturaPA` format
required by the :abbr:`AdE (Agenzia delle Entrate)`. Select the invoices and mark
:guilabel:`Generate XML File` in the :guilabel:`Send and Print` dialog to generate the attachments.

.. image:: italy/italy-edi-menu.png
   :align: center
   :alt: Send and Print menu

.. image:: italy/italy-edi-send-and-print.png
   :align: center
   :alt: Send and Print dialog

.. image:: italy/italy-edi-attachments.png
   :align: center
   :alt: Italian EDI Attachments

2. Submission to SDI
~~~~~~~~~~~~~~~~~~~~

Also from the "Send and Print" window, you can select submission to the :abbr:`SdI (Sistema di
Interscambio)` The invoice is sent to our :guilabel:`Proxy Server`, which gathers all requests and
then forwards them via a WebServices channel to the :abbr:`SdI (Sistema di Interscambio)`. Check the
sending status of the invoice through the appropriate button.

.. image:: italy/italy-edi-check-sending.png
   :align: center
   :alt: Check Sending Button

3. Processing by SDI
~~~~~~~~~~~~~~~~~~~~

The :abbr:`SdI (Sistema di Interscambio)` receives the document and verifies for any errors. At this
stage, the invoice is in the :guilabel:`SdI Processing` state. The checks may take variable time,
ranging from a few seconds to a day, depending on the queue of invoices sent throughout Italy.

.. image:: italy/italy-edi-processing.png
   :align: center
   :alt: EDI Processing State

4. Acceptance
~~~~~~~~~~~~~

If the document is valid, it is recorded and considered fiscally valid by the :abbr:`AdE (Agenzia
delle Entrate)`, which will proceed with archiving in :guilabel:`Substitute Storage (Conservazione
Sostitutiva)` if explicitly requested on the Agency's portal. 

.. warning::
   Odoo does not offer the
   `Conservazione Sostitutiva <https://www.agid.gov.it/index.php/it/piattaforme/conservazione>`_
   requirements. Other providers and :abbr:`AdE (Agenzia delle Entrate)` supply free and certified storage to
   meet the specifications requested by law.

The :guilabel:`SdI address (Codice Destinatario)` will attempt to forward the invoice to the
customer at the provided address, whether it is a `PEC` email address or a :guilabel:`SdI address
(Codice Destinatario)` for their ERP's WebServices channels. A maximum of 6 attempts are made every
12 hours, so even if unsuccessful, this process can take up to three days. The invoice status is
:guilabel:`Accepted by SDI, Forwarding to Partner`.

.. image:: italy/italy-edi-forwarding.png
   :align: center
   :alt: EDI Forwarding State

4b. Possible Rejection
~~~~~~~~~~~~~~~~~~~~~~

The :abbr:`SdI (Sistema di Interscambio)` may find inaccuracies in the compilation, possibly even
formal ones. In this case, the invoice will be in the :guilabel:`SDI Rejected` state. The :abbr:`SdI
(Sistema di Interscambio)`'s observations will be inserted at the top of the Invoice tab. Nothing
serious, it is sufficient to delete the attachments of the invoice, return the invoice to
:guilabel:`Draft`, and fix the errors. Once the invoice is ready, it can be resent.

.. note::
   To regenerate the **XML**, both the **XML** attachment and the **PDF** report must be deleted, so
   that they are then regenerated together. This ensures that both contain the same data.

.. image:: italy/italy-edi-rejected.png
   :align: center
   :alt: EDI Rejected State

5. Forwarding Completed
~~~~~~~~~~~~~~~~~~~~~~~

The invoice has been delivered to the customer; however, remember that many customers still prefer
to have a copy in **PDF** via email or post. Its status is :guilabel:`Accepted by SDI, Delivered to
Partner`.

.. image:: italy/italy-edi-accepted.png
   :align: center
   :alt: EDI Delivered State

5b. Possible Forwarding Failure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the :abbr:`SdI (Sistema di Interscambio)` cannot contact your customer, they may not be
registered on the :abbr:`AdE (Agenzia delle Entrate)` portal. No problem, just make sure to send the
invoice in **PDF** via email or by mail. The invoice will be in the :guilabel:`Accepted by SDI,
Partner Delivery Failed` state.

.. image:: italy/italy-edi-forward-failed.png
   :align: center
   :alt: EDI Forward Failed State

Mastering the Document Types
----------------------------

The :abbr:`SdI (Sistema di Interscambio)` does not only require businesses to send customer invoices
through the :abbr:`EDI (Electronic Data Interchange)` but there are also many other documents that
need sending. All the different configurations are technically identified by a :guilabel:`Document
Type` code. We do **not** show the :guilabel:`Document Type` to the user, but it is used in the
generation of the **XML** file.

TD01 - Invoices
~~~~~~~~~~~~~~~

This represents the standard **domestic** scenario for all invoices exchanged through the :abbr:`SdI
(Sistema di Interscambio)`. Any invoice that doesn't fall into one of the specific special cases
will be categorized as a Regular Invoice, identified by the :guilabel:`Document Type` `TD01`.
  
TD02 - Down payments
~~~~~~~~~~~~~~~~~~~~
  
**Down payment** invoices are imported/exported with a different :guilabel:`Document Type` code
`TDO2` than regular invoices. Upon import of the invoice, it creates a regular vendor bill.

Odoo exports moves as `TD02` if the following conditions are met:

#. Is an invoice;
#. All invoice lines are related to **sales order lines** that have the flag `is_downpayment` set as
   `True`.

TD04 - Credit notes
~~~~~~~~~~~~~~~~~~~

It is the standard scenario for all **credit notes** issued to **domestic** clients, when we need to
formally acknowledge that the seller is reducing or canceling a previously issued invoice, for
example in case of overbilling, incorrect items or overpayment. Just like invoices, they must be
sent to the :abbr:`SdI (Sistema di Interscambio)`, their :guilabel:`Document Type` `TD04`

TD07, TD08, TD09 - Simplified Invoicing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Simplified invoices** (:guilabel:`Document Type` `TD07`), **credit notes** (`TD08`) and **debit
notes** (`TD09`) can be used to certify **domestic transactions** under **400 EUR** (VAT included).
Its status is the same as a regular invoice, but with fewer information requirements.

For a **simplified** invoice to be established, it must include:

#. :guilabel:`Customer Invoice` reference: **unique** numbering sequence with **no gaps**;
#. :guilabel:`Invoice Date`: issue **date** of the invoice;
#. :guilabel:`Company Info`: the **seller**'s full credentials (VAT/TIN number, name, full address)
   under :menuselection:`General Settings --> Companies (section)`;
#. :guilabel:`VAT`: the **buyer**'s VAT/TIN number (on their profile card);
#. :guilabel:`Total`: the total **amount** (VAT included) of the invoice.

In the :abbr:`EDI (Electronic Data Interchange)`, Odoo exports invoices as simplified if:

#. It is a **domestic** transaction (i.e., the partner is from Italy);
#. The buyer's data is **insufficient** for a regular invoice;
#. The **required fields** for a regular invoice (address, ZIP code, city, country) are
   provided;
#. The total amount VAT included is **less** than **400 EUR**.

.. note::
   The 400 EUR threshold was defined in `the decree of the 10th of May 2019 in the Gazzetta
   Ufficiale <https://www.gazzettaufficiale.it/eli/id/2019/05/24/19A03271/sg>`_. We advise you
   to check the current official value.

TD16 - Internal Reverse Charge
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  !! TODO: xxxxx

TD17 - Buying services from abroad
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Buying **services** from **EU** and **non-EU** countries:

The foreign *seller* invoices a service with a **VAT-excluded** price, as it is not
taxable in Italy. The VAT is paid by the *buyer* in Italy;

- Within EU: the *buyer* integrates the invoice received with the **VAT information**
  due in Italy (i.e., **vendor bill tax integration**);
- Non-EU: the *buyer* sends themselves an invoice (i.e., **self-billing**).

Odoo exports a transaction as `TD17` if the following conditions are met:

- Is a vendor bill;
- At least one tax on the invoice lines targets the tax grids :ref:`VJ <italy/grids>`;
- All invoice lines either have :guilabel:`Services` as **products**, or a tax with the
  :guilabel:`Services` as **tax scope**.

TD18 - Buying goods from EU
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Invoices issued within the EU follow a **standard format**, therefore only an integration of
the existing invoice is required.

Odoo exports a transaction as `TD18` if the following conditions are met:

- Is a vendor bill;
- At least one tax on the invoice lines targets the tax grids :ref:`VJ <italy/grids>`;
- All invoice lines either have :guilabel:`Consumable` as **products**, or a tax with the
  :guilabel:`Goods` as **tax scope**.

TD19 - Buying goods from VAT deposit
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Buying **goods** from a **foreign** vendor, but the **goods** are already in **Italy** in a
**VAT deposit**.

- From EU: the *buyer* integrates the invoice received with the **VAT information** due in
  Italy (i.e., **vendor bill tax integration**);
- Non-EU: the *buyer* sends an invoice to *themselves* (i.e., **self-billing**).

Odoo exports a move as a `TD19` if the following conditions are met:

- Is a vendor bill;
- At least one tax on the invoice lines targets the tax grid :ref:`VJ3 <italy/grids>`;
- All invoice lines either have :guilabel:`Consumables` as products, or a tax with
  :guilabel:`Goods` as **tax scope**.

TD24 - Deferred invoices
~~~~~~~~~~~~~~~~~~~~~~~~

The **deferred invoice** is an invoice that is **issued at a later time** than the sale of
goods or the provision of services. A **deferred invoice** has to be issued at the latest
within the **15th day** of the month following the delivery covered by the document.

It usually is a **summary invoice** containing a list of multiple sales of goods or services,
carried out in the month. The business is allowed to **group** the sales into **one invoice**,
generally issued at the **end of the month** for accounting purposes. Deferred invoices are
default for **wholesaler** having recurrent clients.

If the goods are transported by a **carrier**, every delivery has an associated **Documento di
Transporto (DDT)**, or **Transport Document**. The deferred invoice **must** indicate the
details of all the **DDTs** information for better tracing.

.. note::
    E-invoicing of deferred invoices requires the `l10n_it_stock_ddt`
    :ref:`module <italy/modules>`. In this case, a dedicated :guilabel:`Document Type` `TD24`
    is used in the e-invoice.

Odoo exports moves as `TD24` if the following conditions are met:

#. Is an invoice;
#. Is associated to deliveries whose **DDTs** have a **different** date than the issuance date
   of the invoice.

TD28 - San Marino
~~~~~~~~~~~~~~~~~

Invoices
********

San Marino and Italy have special agreements on e-invoicing operations. As such, **invoices** follow
the regular **reverse charge** rules. You can use the proper :guilabel:`Document Type` depending on
the invoice type: `TD01`, `TD04`, `TD05`, `TD24`, `TD25`. Additional requirements are not enforced
by Odoo, however, the user is requested by the **State** to:

- Select a tax with the :guilabel:`Tax Exemption Kind` set to `N3.3`;
- Use the generic :abbr:`SdI (Sistema di Interscambio)` :guilabel:`Destination Code (Codice
  Destinatario)` `2R4GT08`. The invoice is then routed by a dedicated office in San Marino to the
  correct business.

Vendor Bills
************

When a **paper bill** is received from San Marino, any Italian company **must** submit that invoice
to the :abbr:`AdE (Agenzia delle Entrate)` by indicating the e-invoice's :guilabel:`Document Type` field with
the special value `TD28`.

Odoo exports a move as `TD28` if the following conditions are met:
#. Is a vendor bill;
#. At least one tax on the invoice lines targets the tax grids :ref:`VJ <italy/grids>`;
#. The **country** of the partner is **San Marino**.

Public Admnistration Businesses (B2G)
=====================================

.. warning::
   Odoo does **not** send invoices directly to the government as they need to be signed. If we see
   that the codice destinatario is 6 digits, then it is not sent to the PA automatically, but you
   can download the **XML**, sign it with an external program and send it through the portal.

Digital qualified signature
---------------------------

For invoices and bills intended to the **Pubblica Amministrazione (B2G)**, a **Digital Qualified
Signature** is required for all files sent through the :abbr:`SdI (Sistema di Interscambio)`. The
**XML** file must be certified using a certificate that is either:

- a **smart card**;
- a **USB token**;
- a **Hardware Security Module (HSM)**.

CIG, CUP, DatiOrdineAcquisto
----------------------------

To ensure the effective traceability of payments by public administrations, electronic invoices
issued to the public administrations must contain:

- The :abbr:`CIG (Codice Identificativo Gara)`, except in cases of exclusion from traceability
  obligations provided by law n. 136 of August 13, 2010;
- The :abbr:`CUP (Codice Unico di Progetto)`, in case of invoices related to public works.

If the **XML** file requires it, the :abbr:`AdE (Agenzia Delle Entrate)` can *only* proceed payments of
electronic invoices when the **XML** file contains a :abbr:`CIG (Codice Identificativo Gara)` and
:abbr:`CUP (Codice Unico di Progetto)`. For each electronic invoice, it is **necessary** to indicate
the :abbr:`CUU (Codice Univoco Ufficio)`, which represents the unique identifier code that allows
the :abbr:`SdI (Sistema di Interscambio)` to correctly deliver the electronic invoice to the
recipient office.

.. note::
   - The :abbr:`Codice Unico di Progetto)` and the :abbr:`CIG (Codice Identificativo Gara)` must be
     included in one of the **2.1.2** (DatiOrdineAcquisto), **2.1.3** (Dati Contratto), **2.1.4**
     (DatiConvenzione), **2.1.5** (Date Ricezione), or **2.1.6** (Dati Fatture Collegate)
     information blocks. These correspond to the elements named :guilabel:`CodiceCUP` and
     :guilabel:`CodiceCIG` of the electronic invoice **XML** file, whose table can be found on the
     government `website <http://www.fatturapa.gov.it/>`_.
   - The :abbr:`CUU (Codice Univoco Ufficio)` must be included in the electronic invoice
     corresponding to the element **1.1.4** (:guilabel:`CodiceDestinario`).

Process
~~~~~~~

  !! TODO: xxxxxxxxxxxx

#. Requires user signature
#. SdI Accepted, Accepted by the PA Partner
#. SdI Accepted, Rejected by the PA Partner
#. SdI Accepted, PA Partner Expired Terms
