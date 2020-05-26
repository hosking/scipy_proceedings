:author: Zac Hatfield-Dodds
:email: zac.hatfield.dodds@anu.edu.au
:institution: Australian National University

:bibliography: mybib


--------------------------------------------------------------------------
Falsification for Software: effective validation tools for scientific code
--------------------------------------------------------------------------

.. class:: keywords

   property-based testing, validation, verification


.. class:: abstract

    Researchers in all fields are increasingly dependent on software to collect
    and analyse their data


.. This is a methods paper.  My goals are to:
..
.. 1. Convince developers of scientific software that using property-based testing
..    will deliver outcomes they care about, i.e.: more reliable software, without
..    burdensome effort or learning curves.
..
.. 2. Provide an artifact which convinces colleagues and supervisors to support
..    work on property-based testing, i.e. a credible source for the claims above.



Introduction
------------

Much research now depends on software for data collection, analysis, and reporting;
including on software produced and maintained by researchers.  This has empowered
us enormously: it is hard to imagine an analysis that was possible *at all* a
generation ago which could not be acomplished quickly by a graduate student today.

Unfortunately, neither raw computation nor the available software guarantee that
our results are meaningful [#]_.  Indeed, :cite:`Soergel2015` estimates that

    any reported scientific result could very well be wrong if data have passed
    through a computer, and that these errors may remain largely undetected. It is
    therefore necessary to greatly expand our efforts to validate scientific software.

This paper argues that property-based testing :cite:`PraisePBT` is considerably more
effective for validation of Python programs than using only traditional example-based
tests [#]_, without demanding new skills or workflows, and supports this argument
with a variety of examples from well-known scientific Python projects.

.. [#] There is a saying that any fool can make a mistake - but to make billions of mistakes per second, you need a computer.
.. [#] e.g. common workflows using ``pytest``.  If you have no automated tests at all, fix that first, but your second test could reasonably use Hypothesis.


What is property-based testing?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Where example-based tests check for an exact expected output, property-based tests
make *less precise* but *more general* assertions.  By giving up hand-specification
of the expected output, we gain tests that can be run on a wide range of inputs.

Tests which use random data are usually property-based, but using a library like
Hypothesis :cite:`MacIver2019` which is designed for the task has several advantages:

- a concise and expressive interface to describe the range of allowed inputs
- test failures are cached and replayed - never lose a bug, even from other machines
- automatic shrinking, to present a minimal failing example for each distinct error



Link properties to leaky abstractions and managing complexity

shrinking is really really nice - hard to describe how much of a difference it makes

so the advantage is that your data description is concise and general,
and your tests express what you actually care about

empowers you to describe data in rich way, makes reasoning easier.



Why is this so effective?
~~~~~~~~~~~~~~~~~~~~~~~~~



"test-driven research" - combining uncertainty about research domain with
possibly bugs is really really hard to deal with.  Trying to narrow range
of possible reasons for wonky results.

Effective testing lets you focus on your science, not your code.
Analogy to large experimental aparatus - unexpected results mean checking
all of your equipment!

TODO: look at PLYMI testing PR - describing input data is a pain point
most testing is actually really easy once you have good input data

describing core data structures is key - for Numpy arrays, indexers, etc.
also relationships - e.g. compatible shapes (for pytorch, which device data is on)
once you get this right, you don't need to think about feature interactions, they just happen

many contributors are not sophisticated SWEs, so testing is very very valuable
strategies allow you to ask "is all valid data allowed" rather than "are we
covering all the edge cases"

look up mygrad would be impossible without hypothesis

really really vauable to share the sophistication and care that goes into strategies
between all tests, even by novice contributors.





Properties and Case Studies
---------------------------

In this section I present four categories of properties.  While not an exhaustive
list, they are relevant to a very wide range of scientific software - and when
tested often uncover serious errors.

.. TODO explain the structure of subsections, after they have one.


I also present case studies of real-world bugs from the SciPy stack, especially
from Numpy and Astropy.  This is *not* because these packages are more error-prone
than other software - on the contrary, they are of astonishingly high quality
despite catastrophic under-resourcing :cite:`AstropyProblem` :cite:`NumpyFunding`.

The bugs presented below were discovered, reported, and fixed within a few days
thanks to a community-driven and open source development model; and projects from
Astropy to Xarray - via Numpy and Pandas - have begun to adopt property-based tests.



Outputs within expected bounds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
..         Bounded errors, feat. Astropy, Time, and :code:`target()`
..         Conservation laws, physical bounds, and other domain knowledge
.. https://github.com/astropy/astropy/pull/10373


Softmax is a nice example for numerical stability - not arguing that PBT is better
than a stability analysis, rather that the status quo is often that we only
analyse particular routines in isolation.  Empirical checks can also be easier!



Round-trip properties
~~~~~~~~~~~~~~~~~~~~~
..         Does not crash, feat. effective use of assertions, illustrated with np unicode
..         Round-trip properties - esp. IO routines, but also coordinate systems
.. https://github.com/numpy/numpy/issues/15363




Differential testing
~~~~~~~~~~~~~~~~~~~~
..         Differential testing, feat. numpy einsum optimize bug
.. https://github.com/numpy/numpy/issues/10930




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




Metamorphic properties
~~~~~~~~~~~~~~~~~~~~~~
..         Metamorphic properties







Conclusion
----------

Example-based tests provide anecdotal evidence for validity, in that the software
behaves as expected on a few known and typically simple inputs.  Property-based
tests require a precise description of possible inputs and a more general specification,
but support heuristic, guided, and random search for falsifying counter-examples.

I argue that this Popperian approach is superior to the status quo of using only
example-based tests, and hope that the propery-based revolution comes quickly.


Acknowledgements
----------------

Thanks to David MacIver and the many others who have contributed to Hypothesis;
to Ryan Soklaski for his comments on an early draft of this paper; to Anne Archibald
for her work with threshold tests; and to XXXX.
