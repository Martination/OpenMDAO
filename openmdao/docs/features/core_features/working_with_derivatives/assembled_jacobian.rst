.. _feature_assembled_jacobian:

*******************************
In-Memory Assembly of Jacobians
*******************************

When you have groups, or entire models, that are small enough to fit the entire Jacobian into memory, you can have OpenMDAO actually assemble the partial-derivative Jacobian in memory.
In many cases this can yield a substantial speed up over the default, :ref:`matrix-free<theory_assembled_vs_matrix_free>` implementation in OpenMDAO.

.. note::
    Assembled Jacobians are especially effective when you have a deeply-nested hierarchy with a large number of components and/or variables.
    See the :ref:`Theory Manual entry on assembled Jacobians<theory_assembled_vs_matrix_free>` for more details on how to best select which type of Jacobian to use.


To use an assembled Jacobian, you set the :code:`jacobian` attribute of the component or group you would like to have assembled.
There are two kinds of assembled Jacobian you can select from:

   * :ref:`DenseJacobian<openmdao.jacobians.assembled_jacobian.py>`
   * :ref:`CSCJacobian<openmdao.jacobians.assembled_jacobian.py>`

If you are unsure of which one to use, try the :ref:`CSCJacobian<openmdao.jacobians.assembled_jacobian.py>` first.
Most problems, even if they have very dense sub-Jacobians from each component, are fairly sparse at the model level and the :ref:`DirectSolver<directsolver>` will usually be much faster with a sparse factorization.

.. note::

   You are allowed to use multiple assembled Jacobians at multiple different levels of your model hierarchy.
   This may be useful if you have nested non-linear solvers to converge very difficult problems.

-------------
Usage Example
-------------

In the following example, borrowed from the :ref:`newton solver tutorial<defining_icomps_tutorial>`, we assemble the Jacobian at the same level of the model hierarchy as the :ref:`NewtonSolver<nlnewton>` and :ref:`DirectSolver<directsolver>`.
In general, you will always do the assembly at the same level as the linear solver that will make use of the Jacobian matrix.

.. embed-code::
    openmdao.test_suite.test_examples.test_circuit_analysis.TestCircuit.test_circuit_plain_newton_assembled
    :layout: code, output