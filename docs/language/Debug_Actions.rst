Debug Actions
-------------
Curv programs are debugged by inserting ``print`` statements and other debug actions.

Debug actions are statements, but you can embed statements anywhere in a Curv program.
You can attach statements to an expression using a ``do`` expression (see below).
You can embed statements in a set of definitions using a ``test`` definition
(see `<Design_by_Contract.rst>`_).
Debug actions can be embedded in list comprehensions, record comprehensions and
string comprehensions (since a comprehension contains a statement sequence).

``do`` Expressions
~~~~~~~~~~~~~~~~~~
``do actions in expression``
  Execute the ``actions``, then evaluate the ``expression`` and return its result.

For example::

  f x =
      do print "calling function f with argument x=$x";
      in x+1;

Then ``f 2`` returns ``3``, and as a side effect, prints a message
to the debug console.

The *actions* in a ``do`` clause can be a simple debug action, as listed below,
or it can be an imperative style program written using debug actions,
compound statements, ``if`` statements, ``for`` statements, ``while`` statements,
``local`` statements (to define local variables) and assignment statements.
See: `<Statements.rst>`_.

For example, inside a ``do`` clause, you can write a sequence of actions separated by ``;``,
and they will be executed in sequence.

Simple Debug Actions
~~~~~~~~~~~~~~~~~~~~

``print message``
  Print a message string on the debug console, followed by newline.
  If ``message`` is not a string, it is converted to a string using ``repr``.

``warning message``
  Print a message string on the debug console, preceded by "WARNING: ",
  followed by newline and then a stack trace.
  If ``message`` is not a string, it is converted to a string using ``repr``.

``error message``
  On the debug console, print "ERROR: ", then the message string,
  then newline and a stack trace. Then terminate the program.
  If ``message`` is not a string, it is converted to a string using ``repr``.
  An error phrase is legal in either an action context, or in an expression context.
  Example of ``error`` in an expression context::
  
    if (x > 0) f(x) else error "bad x=$x"

``assert condition``
  Evaluate the condition, which must be true or false.
  If it is true, then nothing happens.
  If it is false, then an assertion failure error message is produced,
  followed by a stack trace, and the program is terminated.

``assert_error[error_message_string, expression]``
  Evaluate the expression argument.
  Assert that the expression evaluation terminates with an error,
  and that the resulting error message is equal to ``error_message_string``.
  Used for unit testing.

``exec expression``
  Evaluate the expression and then ignore the result.
  This is used for calling a function whose only purpose is to have a side effect
  (by executing debug actions) and you don't care about the result.

A Note about Pure Functional Semantics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Elsewhere, I claim that Curv is a pure functional language, and that expressions do
not have side effects. This is because debug side effects "don't count" as side effects:
they are only used for debugging, and these side effects cannot change the value computed
by a program. Furthermore, Curv has no globally defined order of evaluation, so there is
no guarantee that messages written to the debug console will occur in the order that you
want. The compiler is free to evaluate independent expressions in any order; it can
execute debug actions at compile time instead of at run time, and it can even ignore
debug actions when compiling for certain execution environments.
