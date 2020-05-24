:author: Zac Hatfield-Dodds
:email: zac.hatfield.dodds@anu.edu.au
:institution: Australian National University

:bibliography: mybib


-----------------------------------------------------
Unreasonably effective validation for scientific code
-----------------------------------------------------
.. or something about Popper / Falsification / scientific method?

.. class:: abstract




   Aiming for two paragramps or so here

.. class:: keywords

   property-based testing, validation, verification


.. ------------------------------------------------------------


This is a methods paper.  My goals are to:

1. Convince developers of scientific software that using property-based testing
   will deliver outcomes they care about, i.e.: more reliable software, without
   burdensome effort or learning curves.

2. Provide an artifact which convinces colleagues and supervisors to support
   work on property-based testing, i.e. a credible source for the claims above.


Paper outline

    Introduction
        Motivation: "did everything right, got wrong results" is a nightmare
        Who is this paper for?  Mostly library authors.
        Example-based vs. property-based testing
        Methods paper, with case studies of general techniques used in Sci Py ecosystem
        Not about blame - great work by overstretched volunteers.  (if it happens to them, it could happen to anyone)

    Properties let you detect bugs without knowing the right answer
        Does not crash, feat. effective use of assertions, illustrated with np unicode
        Bounded errors, feat. Astropy, Time, and :code:`target()`
        Differential testing, feat. numpy einsum optimize bug
        Round-trip properties - esp. IO routines, but also coordinate systems
        Conservation laws, physical bounds, and other domain knowledge
        Metamorphic properties

    Conclusion
        Karl Popper says you should use Hypothesis






















.. code-block:: python

    def test_example():
        p, q = np.ones((10,2)), np.ones((1,2))
        assert_array_equal(
            np.einsum("ij,ij->j", p, q, optimize=True),
            np.einsum("ij,ij->j", p, q, optimize=False)
        )

    @given(
        data=st.data(),
        dtype=npst.integer_dtypes() | npst.floating_dtypes(),
        shape=npst.array_shapes(min_dims=2, max_dims=2),
    )
    def test_property(data, dtype, shape):
        p = data.draw(npst.arrays(dtype, shape))
        q = data.draw(npst.arrays(dtype, shape))
        assert_array_equal( ... )  # as above



















Introduction
------------

:cite:`MacIver2019`





Important Part
--------------

Maecenas [#]_ diam turpis, placerat [#]_ at adipiscing ac, pulvinar id metus.

.. [#] On the one hand, a footnote.
.. [#] On the other hand, another footnote.

.. figure:: figure1.png

   This is the caption. :label:`egfig`

.. figure:: figure1.png
   :align: center
   :figclass: w

   This is a wide figure, specified by adding "w" to the figclass.  It is also
   center aligned, by setting the align keyword (can be left, right or center).

.. figure:: figure1.png
   :scale: 20%
   :figclass: bht

   This is the caption on a smaller figure that will be placed by default at the
   bottom of the page, and failing that it will be placed inline or at the top.
   Note that for now, scale is relative to a completely arbitrary original
   reference size which might be the original size of your image - you probably
   have to play with it. :label:`egfig2`

As you can see in Figures :ref:`egfig` and :ref:`egfig2`, this is how you reference auto-numbered
figures.

.. table:: This is the caption for the materials table. :label:`mtable`

   +------------+----------------+
   | Material   | Units          |
   +============+================+
   | Stone      | 3              |
   +------------+----------------+
   | Water      | 12             |
   +------------+----------------+
   | Cement     | :math:`\alpha` |
   +------------+----------------+


We show the different quantities of materials required in Table
:ref:`mtable`.
