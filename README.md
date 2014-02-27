Handle Double Click at ALV
==========================

This ABAP program was created for teaching purposes. His explanation are into this video:

<a href="http://www.youtube.com/watch?v=3fB9AyT5kQk&list=UU1m92lTepCpEYDu08QYCSrw&feature=c4-overview" target="_blank">Handle Double Click</a>




Installation
------------

Copy the coding and paste to your ABAP environment.



FOLLOW CODING BELLOW:

<div class><pre>

*********************************DECLARAR VARIÁVEIS

DATA it_bnka TYPE TABLE OF bnka.
DATA WA_BNKA TYPE BNKA.

**********************************SELECT NA BNKA
SELECT * UP TO 40 ROWS
  FROM BNKA
  INTO TABLE it_bnka.

************************************MONTAR ALV

  DATA: r_container TYPE REF TO cl_gui_custom_container,
      r_grid TYPE REF TO cl_gui_alv_grid.


DATA: wa_layout   TYPE slis_layout_alv,
      wa_fieldcat TYPE slis_fieldcat_alv,
      it_fieldcat TYPE slis_t_fieldcat_alv.

PERFORM montar_campos.
PERFORM montar_layout.
PERFORM exibir_alv.


FORM montar_campos.

CLEAR wa_fieldcat.

wa_fieldcat-fieldname = 'BANKS'.
wa_fieldcat-tabname   = 'IT_BNKA_ALV'.
wa_fieldcat-seltext_m = 'País'.
APPEND wa_fieldcat TO it_fieldcat.

CLEAR wa_fieldcat.

wa_fieldcat-fieldname = 'BANKL'.
wa_fieldcat-tabname   = 'IT_BNKA_ALV'.
wa_fieldcat-seltext_m = 'Código'.
APPEND wa_fieldcat TO it_fieldcat.

CLEAR wa_fieldcat.

wa_fieldcat-fieldname = 'BANKA'.
wa_fieldcat-tabname   = 'IT_BNKA_ALV'.
wa_fieldcat-seltext_m = 'Banco'.
*  wa_fieldcat-hotspot = 'x'.
APPEND wa_fieldcat TO it_fieldcat.


ENDFORM.



FORM montar_layout.

  wa_layout-zebra = 'X'.
  wa_layout-colwidth_optimize = 'X'.

ENDFORM.


FORM exibir_alv.

  CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY'
  EXPORTING
    i_callback_program = sy-repid
    i_callback_user_command = 'USER_COMMAND'
    it_fieldcat = it_fieldcat
    is_layout = wa_layout
    i_grid_title = 'Relatório de Bancos Importados'

 TABLES

    t_outtab = it_bnka.

ENDFORM.


*************************************************LENDO LINHA QUE VOCÊ CLICOU

FORM user_command USING ucomm LIKE sy-ucomm
                        selfield TYPE kkblo_selfield.
  selfield = selfield.

  CASE ucomm.
    WHEN '&IC1'.
      READ TABLE it_bnka INTO WA_bnka INDEX selfield-tabindex.


SET PARAMETER ID 'BKL' FIELD WA_BNKA-BANKS.
SET PARAMETER ID 'BNK' FIELD WA_BNKA-BANKL.
CALL TRANSACTION 'FI03' AND SKIP FIRST SCREEN.

  ENDCASE.

ENDFORM.
</pre></div>
___________

