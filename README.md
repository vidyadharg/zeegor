==========================================================================

change log...

1. Solution handles call transaction and Batch session(SM35).
2. Demo program Z_BDC_EXAMPLE
3. Installation : Install project via ABAPGit folder source
4. Program ZBDCREC - generates code for SHDB recordings.



![Image1](https://github.com/vidyadharg/zeegor/blob/master/images/image.png)

==========================================================================

Zeegor
======

Simple Batch Programming in ABAP

When developing in an SAP system, sometimes it is helpful or necessary to call a transaction with a batch input table, simulating a user session directly via ABAP.  In the past, this was a verbose, unintuitive process, culminating with `CALL TRANSACTION 'MM03' USING bdc_table`. Want to focus on a screen element? Four lines of code will append that action to your internal table, thank you very much.

Zeegor is a lightweight wrapper to make this process much easier.  Zeegor is your assistant, plugging away at the UI the way you tell him, similar to Dr. Frankenstein's Igor, except with a 'Z' in front of his name because that is in the SAP customer namespace.

Installation
------------
 Install this project via ABAPGit.

Syntax
------
To create a new session, get an instance of `ZCL_BDC_SESSION` using static method `new_transaction( tcode )`.

Then use the following methods to perform UI actions:
`go_to_screen( screen )->in_program( progname )`: These two methods must be chained.
`focus_on( screen_field )`
`fill( screen_field )->with( value )`
`set_okcode( okcode )`

Finally, call `run( )` with all its optional parameters and the transaction will execute.

Example
-------
Here is an example of calling the class builder to display a class.

    REPORT z_bdc_example.
    
    START-OF-SELECTION.
      DATA: bdc TYPE REF TO zcl_bdc_session,
            class_name TYPE c LENGTH 30 VALUE 'CL_SPFLI_PERSISTENT'.
            
      bdc = zcl_bdc_session=>new_transaction( 'SE24' ).
      bdc->go_to_screen( '1000' )->in_program( 'SAPLSEOD' ).
      bdc->focus_on( 'SEOCLASS-CLSNAME' ).
      bdc->fill( 'SEOCLASS-CLSNAME' )->with( class_name ).
      bdc->set_okcode( '=WB_DISPLAY' ).

      bdc->run( screen      = zcl_bdc_session=>screen_show_err_only
                screen_size = zcl_bdc_session=>size_current ).
                  
This same example is in the ABAP documentation as program `DEMO_CALL_TRANSACTION_BDC`, except it is 48 lines long.

Tables and Whatnot
------------------
To focus on or modify a table control, use the method `row( )` like so:

    bdc->fill( 'ITAB-FIELD' )->row( index )->with( my_value ).
    bdc->focus_on( 'ITAB-FIELD' )->row( index + 1 ).
    
Contributing
------------
There are no hard and fast rules for contributing.  If you have a change that is within the scope of BDC, go for it.
