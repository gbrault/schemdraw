.. _electrical:

Basic Circuit Elements
======================

.. jupyter-execute::
    :hide-code:

    %config InlineBackend.figure_format = 'svg'
    import SchemDraw
    from SchemDraw import elements as e
    def drawElements(elm_list, n=5, dx=1, dy=2, ofst=.8, fname=None, **kwargs):
        x, y = 0, 0
        d = SchemDraw.Drawing(fontsize=12)
        for elm in elm_list:
            A = d.add(elm, xy=[(d.unit+1)*x+1,y], label=elm['name'], **kwargs)
            x = x + dx
            if x >= n:
                x=0
                y=y-dy
        d.draw()


These elements are defined in the `SchemDraw.elements` module.

2-terminal Elements
-------------------

Basic Elements
^^^^^^^^^^^^^^

Basic elements define a `start` and `end` anchor for placing.
Depending on the arguments to the `add` method, the leads may be extended
to make the element the desired length.

.. jupyter-execute::
    :hide-code:

    elms = [
        e.RES, e.RES_VAR, e.RBOX, e.CAP, e.CAP2, e.CAP_P, e.CAP2_P, e.CAP_VAR,
        e.INDUCTOR, e.INDUCTOR2, e.DIODE, e.DIODE_F, e.SCHOTTKY,
        e.SCHOTTKY_F, e.DIODE_TUNNEL, e.DIODE_TUNNEL_F, e.ZENER, e.ZENER_F,
        e.LED, e.LED2, e.PHOTODIODE, e.DIAC, e.DIAC_F, e.TRIAC, e.TRIAC_F,
        e.SCR, e.SCR_F, e.FUSE, e.XTAL, e.MEMRISTOR, e.MEMRISTOR2, e.JJ]
    drawElements(elms, n=4, dy=2.25, lblofst=.8, lblloc='center')


Sources and Meters
^^^^^^^^^^^^^^^^^^


.. jupyter-execute::
    :hide-code:

    sources = [e.SOURCE, e.SOURCE_V, e.SOURCE_I, e.SOURCE_SIN, e.SOURCE_CONT,
               e.SOURCE_CONT_I, e.SOURCE_CONT_V, e.BAT_CELL, e.BATTERY,
               e.METER_V, e.METER_I, e.METER_OHM, e.LAMP, e.MOTOR]
    drawElements(sources, n=4, dy=2.25, d='right', lblofst=.2)



Switches
^^^^^^^^

.. jupyter-execute::
    :hide-code:

    switches =[e.SWITCH_SPST, e.SWITCH_SPST_OPEN, e.SWITCH_SPST_CLOSE,
               e.SWITCH_SPDT, e.SWITCH_SPDT_OPEN, e.SWITCH_SPDT_CLOSE,
               e.SWITCH_SPDT2, e.SWITCH_SPDT2_OPEN, e.SWITCH_SPDT2_CLOSE,
               e.BUTTON, e.BUTTON_NC]
    drawElements(switches, n=4, dx=1.4, dy=2.5)#, lblofst=.8)


Labels
^^^^^^

The LABEL element can be used to add a label anywhere.
The GAP_LABEL is like an "invisible" element, useful for marking the voltage between output terminals.

.. jupyter-execute::
    :hide-code:

    d = SchemDraw.Drawing(fontsize=12)
    d.add(e.LINE, d='right', l=1)
    d.add(e.LABEL, xy=[3,-.5], label='LABEL')
    d.add(e.DOT_OPEN)
    d.add(e.GAP_LABEL, d='down', label=['+','GAP_LABEL','$-$'])  # Use math mode to make it a minus, not a hyphen.
    d.add(e.DOT_OPEN)
    d.add(e.LINE, d='left', l=1)
    d.draw()

Other
^^^^^

The microphone and speaker have anchors `in1` and `in2`.

.. jupyter-execute::
    :hide-code:

    other =[e.SPEAKER, e.MIC]
    drawElements(other, n=3, lblloc='center', lblofst=1.1)


Lines, Dots, Arrows
-------------------

.. jupyter-execute::
    :hide-code:

    d = SchemDraw.Drawing(fontsize=12)
    d.add(e.LINE, l=4, label='LINE')
    d.add(e.DOT, label='DOT')
    d.add(e.LINE, l=2)
    d.add(e.DOT_OPEN, label='DOT_OPEN')
    d.add(e.LINE, l=3)
    d.add(e.ARROWHEAD, label='ARROWHEAD')
    d.draw()


1-terminal elements
-------------------

One-terminal elements do not move the current drawing position, and ignore any `add` parameters
that specify an endpoint.

.. jupyter-execute::
    :hide-code:

    grounds = [e.GND, e.GND_SIG, e.GND_CHASSIS, e.VSS, e.VDD, e.ANT]
    drawElements(grounds, n=3, dy=3)


3-terminal Elements
-------------------

Three terminal elements define anchor names so that any of the three terminals can
be placed at the desired drawing position.

Potentiometer is defined with one additional anchor for the 'tap':

.. jupyter-execute::
    :hide-code:

    d = SchemDraw.Drawing(fontsize=12)
    P = d.add(e.POT, botlabel='POT')
    P.add_label('tap', loc='tap')
    d.add(e.GAP_LABEL, d='up', l=.5)
    d.draw()


BJT and FET transistors also define three anchors:

.. jupyter-execute::
    :hide-code:

    d = SchemDraw.Drawing(fontsize=12)
    bjt = d.add(e.BJT_NPN, xy=[0, 0], anchor='base')
    bjt.add_label('base', loc='base', align=('right', 'center'), ofst=[-.1, 0])
    bjt.add_label('emitter', loc='emitter', align=('center', 'top'), ofst=-.2)
    bjt.add_label('collector', loc='collector')

    fet = d.add(e.NFET, xy=[3, 0], anchor='gate', d='left')
    fet.add_label('gate', loc='gate', ofst=[.1, 0], align=('right', 'center'))
    fet.add_label('source', loc='source', align=('center', 'bottom'), ofst=-.1)
    fet.add_label('drain', loc='drain', align=('center', 'top'))
    d.draw()

Names of the different transistor elements are shown below:

.. jupyter-execute::
    :hide-code:

    bjt = [e.BJT,e.BJT_NPN,e.BJT_PNP,e.BJT_NPN_C,e.BJT_PNP_C,e.BJT_PNP_2C]
    drawElements(bjt, n=3, dy=2.5, lblloc='top')

.. jupyter-execute::
    :hide-code:

    d = SchemDraw.Drawing(fontsize=12)
    d.add(e.NFET, label='NFET', lblloc='top')
    d.add(e.PFET, label='PFET', lblloc='top', xy=[3,0] )
    d.add(e.NFET4, label='NFET4', lblloc='top', xy=[6,0])
    d.add(e.PFET4, label='PFET4', lblloc='top', xy=[9,0])
    d.add(e.JFET_N, label='JFET_N', lblloc='top', xy=[0,-3])
    d.add(e.JFET_P, label='JFET_N', lblloc='top', xy=[3,-3])
    d.add(e.JFET_N_C, label='JFET_N_C', lblloc='top', xy=[6,-3])
    d.add(e.JFET_P_C, label='JFET_N_C', lblloc='top', xy=[9,-3])
    d.draw()

An opamp defines anchors `in1`, `in2`, and `out`, plus `vd`, `vs` for supply voltages and `n1`, `n2`, `n1a`, `n2a` for offset inputs.

.. jupyter-execute::
    :hide-code:
    
    d = SchemDraw.Drawing(fontsize=12)
    op = d.add( e.OPAMP, label='OPAMP', lblofst=.6)
    d.add(e.LINE, xy=op.in1, d='left', l=.5, lftlabel='in1')
    d.add(e.LINE, xy=op.in2, d='left', l=.5, lftlabel='in2')
    d.add(e.LINE, xy=op.out, d='right', l=.5, rgtlabel='out')
    d.add(e.LINE, xy=op.vd, d='up', l=.25, rgtlabel='vd')
    d.add(e.LINE, xy=op.vs, d='down', l=.25, lftlabel='vs')    
    d.add(e.LINE, xy=op.n2, d='up', l=.25, rgtlabel='n2')
    d.add(e.LINE, xy=op.n1, d='down', l=.25, lftlabel='n1')    
    d.add(e.LINE, xy=op.n2a, d='up', l=.22, rgtlabel='n2a', lblofst=0)
    d.add(e.LINE, xy=op.n1a, d='down', l=.22, lftlabel='n1a', lblofst=0)    
    
    op2 = d.add(e.OPAMP_NOSIGN, xy=[5, 0], d='right', label='OPAMP_NOSIGN', lblofst=.6)
    d.add(e.LINE, xy=op2.in1, d='left', l=.5, lftlabel='in1')
    d.add(e.LINE, xy=op2.in2, d='left', l=.5, lftlabel='in2')
    d.add(e.LINE, xy=op2.out, d='right', l=.5, rgtlabel='out')
    d.add(e.LINE, xy=op2.vd, d='up', l=.25, rgtlabel='vd')
    d.add(e.LINE, xy=op2.vs, d='down', l=.25, lftlabel='vs')    
    d.add(e.LINE, xy=op2.n2, d='up', l=.25, rgtlabel='n2')
    d.add(e.LINE, xy=op2.n1, d='down', l=.25, lftlabel='n1')    
    d.add(e.LINE, xy=op2.n2a, d='up', l=.22, rgtlabel='n2a', lblofst=0)
    d.add(e.LINE, xy=op2.n1a, d='down', l=.22, lftlabel='n1a', lblofst=0)      
    d.draw()


Transformers
------------

Transformer elements can be generated using the :py:func:`SchemDraw.elements.transformer` function.

.. function:: SchemDraw.elements.transformer(t1=4, t2=4, core=True, ltaps=None, rtaps=None, loop=False)

   Generate an element definition for a transformer

   :param t1: turns on left side
   :type t1: int
   :param t2: turns on right side
   :type t2: int
   :param core: show the transformer core
   :type core: bool
   :param ltaps: anchor definitions for left side. Each key/value pair defines the name/turn number
   :type ltaps: dict
   :param rtaps: anchor definitions for right side.
   :type rtaps: dict
   :param loop: Use spiral/cycloid (loopy) style
   :type loop: bool
   :returns: element definition dictionary
   :rtype: dict


Two transformers with cycloid=False (left) cycloid=True (right) shown below. Anchor names are `p1` and `p2` for the primary (left) side,
and `s1` and `s2` for the secondary (right) side.

.. jupyter-execute::
    :hide-code:

    d = SchemDraw.Drawing()
    x = d.add(e.transformer(6,3, core=True, loop=False))
    d.add(e.LINE, xy=x.s1, l=d.unit/4)
    d.add(e.LINE, xy=x.s2, l=d.unit/4)
    d.add(e.LINE, xy=x.p1, l=d.unit/4, d='left')
    d.add(e.LINE, xy=x.p2, l=d.unit/4, d='left')

    x2 = d.add(e.transformer(6,3, core=False, loop=True), d='right', xy=(4,0))
    d.add(e.LINE, xy=x2.s1, l=d.unit/4, d='right')
    d.add(e.LINE, xy=x2.s2, l=d.unit/4, d='right')
    d.add(e.LINE, xy=x2.p1, l=d.unit/4, d='left')
    d.add(e.LINE, xy=x2.p2, l=d.unit/4, d='left')
    d.draw()

Example usage with taps:

.. jupyter-execute::

    d = SchemDraw.Drawing()
    xf = d.add( e.transformer(t1=4, t2=8, rtaps={'B':3}, loop=False ) )
    d.add(e.LINE, xy=xf.s1, l=d.unit/4, rgtlabel='s1')
    d.add(e.LINE, xy=xf.s2, l=d.unit/4, rgtlabel='s2')
    d.add(e.LINE, xy=xf.p1, l=d.unit/4, d='left', lftlabel='p1')
    d.add(e.LINE, xy=xf.p2, l=d.unit/4, d='left', lftlabel='p2')
    d.add(e.LINE, xy=xf.B, l=d.unit/2, d='right', rgtlabel='B')
    d.draw()


Integrated Circuits
-------------------

Elements drawn as boxes, such as integrated circuits, can be generated using the :py:func:`SchemDraw.elements.ic` function.
An arbitrary number of inputs/outputs can be drawn to each side of the box.
The inputs can be evenly spaced (default) or arbitrarily placed anywhere along each edge.

.. function:: SchemDraw.elements.ic(*pins, **kwargs)

    Define an integrated circuit element

    :param pins: Dictionaries defining each input pin entered as positional arguments
    
    :pins dictionary:
        * **name**: (string) Signal name, labeled inside the IC box. If name is '>', a proper clock input triangle will be drawn instead of a text label.
        * **pin**: (string) Pin name, labeled outside the IC box
        * **side**: ['left', 'right', 'top', 'bottom']. Which side the pin belongs on. Can be abbreviated 'L', 'R', 'T', or 'B'.
        * **pos**: (float) Absolute position as fraction from 0-1 along the side. If not provided, pins are evenly spaced along the side.
        * **slot**: (string) Position designation for the pin in "X/Y" format where X is the pin number and Y the total number of pins along the side. Use when missing pins are desired with even spacing.
        * **invert**: (bool) Draw an invert bubble outside the pin
        * **invertradius**: (float) Radius of invert bubble
        * **color**: (string) Matplotlib color for label
        * **rotation**: (float) Rotation angle for label (degrees)
        * **anchorname**: (string) Name of anchor at end of pin lead. By default pins will have anchors of both the `name` parameter  and `inXY` where X the side designation ['L', 'R', 'T', 'B'] and Y the pin number along that side.

    :Keyword Arguments:
        * **size**: (w, h) tuple specifying size of the IC. If not provided, size is automatically determined based on number of pins and the pinspacing parameter.
        * **pinspacing**: Smallest distance between pins [1.25]
        * **edgepadH**, **edgepadW**: Additional distance from edge to first pin on each sides [.25]
        * **lblofst**: Default offset for (internal) labels [.15]
        * **plblofst**: Default offset for external pin labels [.1]
        * **leadlen**: Length of leads extending from box [.5]
        * **lblsize**: Font size for (internal) labels [14]
        * **plblsize**: Font size for external pin labels [11]
        * **slant**: Degrees to slant top and bottom sides (e.g. for multiplexers) [0]


Here, a J-K flip flop, as part of an HC7476 integrated circuit, is drawn with input names and pin numbers.

.. jupyter-execute::
    :hide-code:
    
    d = SchemDraw.Drawing()

.. jupyter-execute::
    :hide-output:

    linputs = {'labels':['>', 'K', 'J'], 'plabels':['1', '16', '4']}
    rinputs = {'labels':['$\overline{Q}$', 'Q'], 'plabels':['14', '15']}
    JKdef = e.ic({'name': '>', 'pin': '1', 'side': 'left'},
                 {'name': 'K', 'pin': '16', 'side': 'left'},
                 {'name': 'J', 'pin': '4', 'side': 'left'},
                 {'name': '$\overline{Q}$', 'pin': '14', 'side': 'right', 'anchorname': 'QBAR'},
                 {'name': 'Q', 'pin': '15', 'side': 'right'},
                 edgepadW = .5  # Make it a bit wider
                 )
    JK = d.add(JKdef, label='HC7476', lblsize=12, lblofst=.5)

.. jupyter-execute::
    :hide-code:
    
    d.draw()

Notice the use of `$\overline{Q}$` to acheive the label on the inverting output.
The anchor positions can be accessed using attributes, such as `JK.Q` for the
non-inverting output. However, inverting output is named `$\overline{Q}`, which is
not accessible using the typical dot notation. It could be accessed using 
`getattr(JK, '$\overline{Q}$')`, but to avoid this an alternative anchorname of `QBAR`
was defined.


Multiplexers
^^^^^^^^^^^^

Multiplexers and demultiplexers may be drawn using the :py:func:`SchemDraw.elements.mux` function which wraps the :py:func:`SchemDraw.elements.ic` function.

.. function:: SchemDraw.elements.multiplexer(*pins, demux=False, **kwargs)
        
        Define a multiplexer or demultiplexer element
    
        :param pins: Pin definition dictionaries. See :py:func:`SchemDraw.elements.ic`.
        
        :Keyword Arguments:
            * demux: (bool) Draw demultiplexer (opposite slope)
            * \**kwargs: See :py:func:`SchemDraw.elements.ic`.

.. jupyter-execute::
    :hide-code:
    
    d = SchemDraw.Drawing()

.. jupyter-execute::
    :hide-output:

    m1 = e.multiplexer({'name': 'C', 'side': 'L'},
                       {'name': 'B', 'side': 'L'},
                       {'name': 'A', 'side': 'L'},
                       {'name': 'Q', 'side': 'R'},
                       {'name': 'T', 'side': 'B', 'invert':True},
                       edgepadH=-.5)
    d.add(m1)

.. jupyter-execute::
    :hide-code:
    
    d.draw()
