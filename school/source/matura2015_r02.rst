Zadanie 02 - 
------------

.. image:: matura2015/matura2015_r02.png
   :align: center

Poniższy kod:


.. sagecellserver::

    forget()
    assume(x<=0)
    print len(solve(x-2==1,x))
    forget()
    assume(x>0)
    print len(solve((abs(x+3)-4)==1,x))


