.. _flintxx_functions:

**flintxx functions** -- C++ Wrapper
===============================================================================

Flint exception type
===============================================================================

    flint\_exception

    This is the main exception type used by the flintxx library. It derives
    from ``std::domain_error``. As such its main method is ``what()``,
    yielding an English language description of the problem encountered.

Random functions
===============================================================================

.. type:: frandxx

    The type ``frandxx`` wraps ``flint_rand_t`` and takes care of
    initialising and clearing random states. It is defined in
    ``flintxx/frandxx.h``. Note that this type is not copyable.

.. function:: frandxx::frandxx()

    Initialize random state.

.. function:: flint_rand_t& frandxx::_data()
.. function:: const flint_rand_t& frandxx::_data() const

    Obtain a reference to the underlying C random state.

Tuples
===============================================================================

.. type:: ltuple

    Lazy tuples are implemented in ``flintxx/ltuple.h``. They are used
    throughout flintxx to emulate functions with several return values.

    This header automatically creates a static instance of
    ``flint::detail::IGNORED_TYPE``. It is accessible in namespace flint,
    under the name ``FLINT_LTUPLE_PLACEHOLDER_NAME``, which defaults to
    ``_``. See ``ltupleref`` documentation for how to use this.

    Construct an ltuple of references, binding to the arguments ``t1``,
    \dots, ``tn``. Instances of ``flint::detail::IGNORED_TYPE`` can be
    used as placeholders.
    Currently $n \le 4$.

.. function:: Ltuple<T1, ..., Tn> ltuple(const T1& t1, ..., const Tn& tn)

    Construct an ltuple containing copies of ``t1``, \dots, ``tn``.
    Currently \code{n \le 4}.

.. function:: Tk_expr Ltuple<T1, ..., Tn>_expr::get<k>() const

    If ``Tk`` is an expression template type, then the ``get<k>()``
    method returns a lazy expression evaluating to the kth element of the
    (potentially lazy) ltuple.

    If ``Tk`` is not an expression template type, this method evaluates the
    ltuple, and returns the kth entry.

    On ltuple immediates, reference versions are also available, which can
    be used to manipulate the entries.

Permutations
===============================================================================

.. type:: permxx

    Permutations are mostly used by row reduction algorithms. Even though we
    support limited arithmetic on them (e.g. composition), permutations are not
    implemented as expression templates.

    ``permxx`` wraps the C interface ``perm`` operating on ``slong*``.

.. function:: permxx::permxx(slong n)
.. function:: static permxx permxx::one(slong n)

    Initialize an identity permutation on the set $[n] = \{0, 1, \dots, n-1\}$.

.. function:: static permxx permxx::randtest(slong n)

    Generate a random permutation on $[n]$. See ``_perm_randtest``.

.. function:: bool permxx::operator==(const permxx&)
.. function:: bool permxx::operator!=(const permxx&)

.. function:: slong permxx::size() const

    Return the size of the set being permuted ($n$ in the constructors).

.. function:: slong& operator[](slong i)
.. function:: slong operator[](slong i) const

    Return the image of $i$ under the permutation.

.. function:: permxx permxx::operator*(const permxx&)
.. function:: permxx compose(const permxx& p1, const permxx& p2)

    Compute the composition of two permutations. See ``_perm_compose``.

.. function:: void permxx::set_inv(const permxx& o)

    Set self to the inverse permutation of ``o``.

.. function:: permxx permxx::inv() const
.. function:: permxx inv(const permxx&)

    Return the inverse permutation.

.. function:: int print(const permxx&)

Integers (``fmpz``)
===============================================================================

.. type:: fmpzxx

C++ particulars
-------------------------------------------------------------------------------

.. function:: Fmpz_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``sqrt``, ``abs``.

.. function:: Fmpz_expr::binary operation(??) const

    The following binary functions are made available as member functions:
    ``cdiv_q``, ``divexact``, ``fdiv_qr``, ``fdiv_r``,
    ``fdiv_r_2exp``, ``gcd``, ``gcdinv``, ``invmod``, ``lcm``,
    ``negmod``, ``pow``, ``rfac``, ``root``, ``sqrtmod``,
    ``tdiv_q``, ``tdiv_q_2exp``, ``tdiv_qr``, ``xgcd``.

.. function:: Fmpz_expr::ternary operation(??, ??) const

    The following ternary functions are made available as member functions:
    ``divexact2``, ``mul2``, ``mul_tdiv_q_2exp``, ``powm``.

Memory management
-------------------------------------------------------------------------------

.. function:: fmpzxx::fmpzxx()

    Initialize to zero.

.. function:: fmpzxx::fmpzxx(const char*)
.. function:: fmpzxx::fmpzxx(T:is_integer)

    Initialize from a primitive data type. See ``fmpz_set_str``,
    ``fmpz_set_si`` and ``fmpz_set_ui``.

Random generation
-------------------------------------------------------------------------------

.. function:: static fmpzxx fmpzxx::randbits(frandxx& state)

.. function:: static fmpzxx fmpzxx::randtest(frandxx& state)

.. function:: static fmpzxx fmpzxx::randtest_unsigned(frandxx& state)

.. function:: static fmpzxx fmpzxx::randtest_not_zero(frandxx& state)

.. function:: static fmpzxx fmpzxx::randm(frandxx& state, Fmpz_expr m)

.. function:: static fmpzxx fmpzxx::randtest_mod(frandxx& state, Fmpz_expr m)

.. function:: static fmpzxx fmpzxx::randtest_mod_signed(frandxx& state, Fmpz_expr m)

Conversion
-------------------------------------------------------------------------------

.. function:: std::string Fmpz_expr::to_string(int base = 10) const

    Convert self into a ``string``. See ``fmpz_get_str``.

.. function:: slong Fmpz_expr::to<slong>() const

    Convert self to ``slong``. See ``fmpz_get_si``.

.. function:: ulong Fmpz_expr::to<ulong>() const

    Convert self to ``ulong``. See ``fmpz_get_ui``.

.. function:: double Fmpz_expr::to<double>() const

    Convert self to ``double``. See ``fmpz_get_d``.

.. function:: double Fmpz_expr::get_d_2exp(long& exp) const

.. function:: Fmpz_target Fmpz_target::operator=(const char*)
.. function:: Fmpz_target Fmpz_target::operator=(T:is_integer)

    See ``fmpz_set_str``, ``fmpz_set_ui`` and ``fmpz_set_si``.

.. function:: void Fmpz_target::set_ui_smod(mp_limb_t x, mv_limb_t m)

.. function:: void Fmpz_target::set_uiui(mp_limb_t x, mv_limb_t m)

.. function:: void Fmpz_target::neg_uiui(mp_limb_t x, mv_limb_t m)

Input and output
-------------------------------------------------------------------------------

.. function:: int print(Fmpz_expr)
.. function:: int print(FILE*, Fmpz_expr)
.. function:: int read(Fmpz_target)
.. function:: int read(FILE*, Fmpz_target)

Basic properties and manipulation
-------------------------------------------------------------------------------

.. function:: size_t Fmpz_expr::sizeinbase(int) const
.. function:: size_t sizeinbase(Fmpz_expr, int)
.. function:: mp_bitcnt_t Fmpz_expr::bits() const
.. function:: mp_bitcnt_t bits(Fmpz_expr)
.. function:: mp_bitcnt_t Fmpz_expr::size() const
.. function:: mp_bitcnt_t size(Fmpz_expr)
.. function:: mp_bitcnt_t Fmpz_expr::val2() const
.. function:: mp_bitcnt_t val2(Fmpz_expr)
.. function:: int Fmpz_expr::sign() const
.. function:: int sign(Fmpz_expr)

.. function:: void Fmpz_target::set_zero()
.. function:: void Fmpz_target::set_one()

.. function:: bool Fmpz_expr::abs_fits_ui() const
.. function:: bool Fmpz_expr::fits_si() const

.. function:: void Fmpz_target::setbit(ulong)
.. function:: bool Fmpz_expr::tstbit(ulong) const

Comparison
-------------------------------------------------------------------------------

Relational operators ``<=``, ``>`` etc. are overloaded, where ``e1``
and ``e2`` can be any combination of ``Fmpz_expr`` and
``T:is_integer``.

See ``fmpz_cmp``, ``fmpz_cmp_si`` and ``fmpz_cmp_ui``.

.. function:: bool Fmpz_expr::is_zero() const

    Return if this expression evaluates to zero.

.. function:: bool Fmpz_expr::is_one() const

    Return if this expression evaluates to one.

.. function:: bool Fmpz_expr::is_pm1() const

    Return if this expression evaluates to $\pm 1$.

.. function:: bool Fmpz_expr::is_even() const

    Return if this expression evaluates to an even integer.

.. function:: bool Fmpz_expr::is_odd() const

    Return if the expression evaluates to an odd integer.

Basic arithmetic
-------------------------------------------------------------------------------

Arithmetic operators ``+``, ``-``, ``*``, ``/``, ``%``,
``<<`` and ``>>`` are overloaded. See the ``fmpz`` documentation for
which argument types are allowed. Symmetric operators with asymmetric type
arguments can be used in either order, even if this is not exposed in the C
interface.

The shift operators wrap ``fmpz_fdiv_q_2exp`` and ``fmpz_mul_2exp``.

The division operators use ``fmpz_fdiv``.

.. function:: Fmpz_expr abs(Fmpz_expr)

.. function:: Fmpz_expr mul2_uiui(Fmpz_expr g, ulong x, ulong y)

.. function:: Fmpz_expr cdiv_q(Fmpz_expr, Fmpz_expr)
.. function:: Fmpz_expr cdiv_q(Fmpz_expr, T:is_integer)

.. function:: Fmpz_expr tdiv_q(Fmpz_expr, Fmpz_expr)
.. function:: Fmpz_expr tdiv_q(Fmpz_expr, T:is_integer)

.. function:: Fmpz_expr divexact(Fmpz_expr, Fmpz_expr)
.. function:: Fmpz_expr divexact(Fmpz_expr, T:is_integer)

.. function:: Fmpz_expr fdiv_r(Fmpz_expr, Fmpz_expr)

.. function:: Fmpz_expr tdiv_q_2exp(Fmpz_expr, T:is_unsigned_integer)

.. function:: Fmpz_expr fdiv_r_2exp(Fmpz_expr, T:is_unsigned_integer)

.. function:: Fmpz_expr divexact2(Fmpz_expr g, ulong x, ulong y)

.. function:: Fmpz_expr mul_tdiv_q_2exp(Fmpz_expr g, Fmpz_expr x, ulong exp)
.. function:: Fmpz_expr mul_tdiv_q_2exp(Fmpz_expr g, long x, ulong exp)

.. function:: Ltuple<fmpzxx, fmpzxx>_expr fdiv_qr(Fmpz_expr g, Fmpz_expr h)
.. function:: Ltuple<fmpzxx, fmpzxx>_expr tdiv_qr(Fmpz_expr g, Fmpz_expr h)

.. function:: bool Fmpz_expr::divisible(Fmpz_expr) const
.. function:: bool Fmpz_expr::divisible(T:fits_into_slong) const
.. function:: bool divisible(Fmpz_expr n, Fmpz_expr d)
.. function:: bool divisible(Fmpz_expr n, T:fits_into_slong d)

    Return if $d$ divides $n$. See ``fmpz_divisible``.

.. function:: Fmpz_expr powm(Fmpz_expr g, ulong e, Fmpz_expr m)
.. function:: Fmpz_expr powm(Fmpz_expr g, Fmpz_expr e, Fmpz_expr m)

.. function:: Fmpz_expr pow(Fmpz_expr, T:is_unsigned_integer)

.. function:: long clog(Fmpz_expr x, Fmpz_expr b)
.. function:: long clog(Fmpz_expr x, ulong b)

.. function:: long flog(Fmpz_expr x, Fmpz_expr b)
.. function:: long flog(Fmpz_expr x, ulong b)

.. function:: double dlog(Fmpz_expr x)

.. function:: long Fmpz_expr::clog(Fmpz_expr) const
.. function:: long Fmpz_expr::clog(T:is_unsigned_integer) const

.. function:: long Fmpz_expr::flog(Fmpz_expr) const
.. function:: long Fmpz_expr::flog(T:is_unsigned_integer) const

.. function:: double Fmpz_expr::dlog() const

.. function:: Ltuple<bool, fmpzxx>_expr sqrtmod(Fmpz_expr a, Fmpz_expr b)

    ``ltupleref(b, N) = sqrtmod(A, B)`` has the same effect as
    ``b = fmpz_sqrtmod(n, a, b)``, where ``n, a, b`` are the underlying
    ``fmpz_t`` of ``N, A, B``.

.. function:: Ltuple<fmpzxx, fmpzxx>_expr sqrtrem(Fmpz_expr g)

.. function:: Fmpz_expr sqrt(Fmpz_expr)

.. function:: bool Fmpz_expr::is_square() const

    Return if this expression evaluates to a square integer.

.. function:: Fmpz_expr root(Fmpz_expr, T:fits_into_slong)

.. function:: Fmpz_expr rfac(Fmpz_expr, T:is_unsigned_integer)

.. function:: Fmpz_expr fac(T:is_unsigned_integer)

.. function:: Fmpz_expr fib(T:is_unsigned_integer)

.. function:: Fmpz_expr bin(T:is_unsigned_integer, U:is_unsigned_integer)

Greatest common divisor
-------------------------------------------------------------------------------

.. function:: Ltuple<fmpzxx, fmpzxx>_expr gcdinv(Fmpz_expr f, Fmpz_expr g)

.. function:: Ltuple<fmpzxx, fmpzxx, fmpzxx>_expr xgcd(Fmpz_expr f, Fmpz_expr g)

.. function:: Fmpz_expr gcd(Fmpz_expr, Fmpz_expr)

.. function:: Fmpz_expr lcm(Fmpz_expr, Fmpz_expr)

Modular arithmetic
-------------------------------------------------------------------------------

.. function:: Ltuple<slong, fmpzxx>_expr remove(Fmpzxx a, Fmpzxx b)

.. function:: int jacobi(Fmpz_expr a, Fmpz_expr p)
.. function:: int Fmpz_expr::jacobi(Fmpz_expr) const

.. function:: Fmpz_expr invmod(Fmpz_expr, Fmpz_expr)

.. function:: Fmpz_expr negmod(Fmpz_expr, Fmpz_expr)

Bit packing and unpacking
-------------------------------------------------------------------------------

Beware that argument orders are different relative to the C interface, to
facilitate default arguments.

.. function:: static Fmpz_expr fmpzxx::bit_unpack(const vector<mp_limb_t>& v, mp_bitcnt_t bits, mp_bitcnt_t shift = 0, int negate = 0, bool borrow = false)
.. function:: static Fmpz_expr fmpzxx::bit_unpack_unsigned(const vector<mp_limb_t>& v, mp_bitcnt_t bits, mp_bitcnt_t shift = 0)

    Unpack an ``fmpzxx`` from ``v``.

.. function:: bool bit_pack(std::vector<mp_limb_t>& v, mp_bitcnt_t bits, Fmpz_expr, mp_bitcnt_t shift = 0, int negate = 0, bool borrow = false)

    Pack an ``fmpzxx`` to ``v``. The vector ``v`` is required to be of
    sufficient size.

Logic operations
-------------------------------------------------------------------------------

Binary logic operators ``& | ^`` (and, or, xor) are also overloaded
(implemented when both arguments are ``Fmpz_expr``).

.. function:: void Fmpz_target::clrbit(ulong i)

.. function:: void Fmpz_target::combit(ulong i)

.. function:: int Fmpz_expr::popcnt() const

Chinese remaindering
-------------------------------------------------------------------------------

.. function:: Fmpz_expr Fmpz_expr::CRT(Fmpz_expr, T:is_unsigned_integer, T:is_unsigned_integer, bool) const
.. function:: Fmpz_expr CRT(Fmpz_expr, Fmpz_expr, T:is_unsigned_integer, T:is_unsigned_integer, bool)

    See ``fmpz_CRT_ui``.

.. function:: fmpz_combxx::fmpz_combxx(const std::vector<mp_limb_t>& primes)

    The class ``fmpz_combxx`` wraps both ``fmpz_comb_t`` and
    ``fmpz_comb_temp_t``. The argument ``primes`` is the vector of moduli
    to use, and must not be deallocated before the newly constructed
    ``fmpz_combxx``. Note that the internal ``fmpz_comb_temp_t``
    structure may be modified even on constant instances of ``fmpz_combxx``.

.. function:: void multi_mod(std::vector<mp_limb_t>& out, Fmpz_expr in, const fmpz_combxx& comb)

    Reduce ``in`` modulo the primes stored in ``comb``, and store the
    results in ``out``. The vector ``out`` must have sufficient size, and
    its size will not be changed.

.. function:: Fmpz_expr multi_CRT(const std::vector<mp_limb_t>& residues, const fmpz_combxx comb, bool sign)

    Reconstruct an integer from its residues. See ``fmpz_multi_CRT_ui``.

Primality testing
-------------------------------------------------------------------------------

.. function:: bool Fmpz_expr::is_probabprime() const

.. function:: bool Fmpz_expr::is_prime_pseudosquare() const


Integer factoring (``fmpz_factor``)
===============================================================================

.. function:: fmpz_factorxx::fmpz_factorxx()

    Initialise an empty factorisation.

.. function:: fmpz_factorxx::fmpz_factorxx(const fmpz_factorxx& o)

    Copy a factorisation.

.. function:: bool fmpz_factorxx::operator==(const fmpz_factorxx&)

    Compare two factorisations.

.. function:: ulong fmpz_factorxx::size() const

    Return the number of stored factors.

.. function:: ulong fmpz_factorxx::exp(slong i) const
.. function:: ulong& fmpz_factorxx::exp(slong i)

    Obtain the exponent of the ith factor.

.. function:: fmpzxx_srcref fmpz_factorxx::p(slong i) const
.. function:: fmpzxx_ref fmpz_factorxx::p(slong i)

    Obtain the ith factor.

.. function:: int fmpz_factorxx::sign() const
.. function:: int& fmpz_factorxx::sign()

    Obtain the sign of the factored expression.

.. function:: void fmpz_factorxx::set_factor(Fmpz_expr)
.. function:: void fmpz_factorxx::set_factor(T:fits_into_slong)
.. function:: bool fmpz_factorxx::set_factor_trial_range(Fmpz_expr, ulong, ulong)
.. function:: bool fmpz_factorxx::set_factor_pp1(Fmpz_expr, ulong, ulong, ulong)

    Factorise an integer and store its factors. See ``fmpz_factor`` etc.

.. function:: Fmpz_expr fmpz_factorxx::expand() const
.. function:: Fmpz_expr fmpz_factorxx::expand_iterative() const
.. function:: Fmpz_expr fmpz_factorxx::expand_multiexp() const

.. function:: fmpz_factorxx factor(Fmpz_expr)
.. function:: fmpz_factorxx factor(T:fits_into_slong)
.. function:: Ltuple<bool, fmpz_factorxx>_expr factor_trial_range(Fmpz_expr)
.. function:: fmpz_factorxx factor_pp1(Fmpz_expr)

.. function:: void print(const fmpz_factorxx&)

Integer matrices (``fmpz_mat``)
===============================================================================

.. type:: fmpz_matxx

The class ``fmpz_matxx`` wraps ``fmpz_mat_t``, and so represents matrices
with coefficients in $\mathbf{Z}$.

Owing to the design of ``fmpz_mat_t``, the use of ``fmpz_matxx`` has a
number of peculiarities.

- Matrix assignment does not automatically resize. This also includes assigning
  (and thus evaluating) a lazy expression to an ordinary matrix. As a
  consequence, the evaluation code cannot use temporary merging, and may thus
  create more temporaries than a similar expression involving non-matrices.

- Several functions operating on ``fmpz_mat_t`` do not allow aliasing. The
  flintxx layer just passes expressions on to the C layer, so it is the
  responsibility of the user to avoid aliasing where it is disallowed. Note
  that since no temporary merging is used with matrices, aliases are never
  introduced by the evaluation code.

Miscellaneous
-------------------------------------------------------------------------------

.. function:: slong Fmpz_mat_expr::rank() const

.. function:: Fmpz_expr Fmpz_mat_expr::det_modular_given_divisor(
        Fmpz_mat_expr, Fmpz_expr) const

    See ``fmpz_mat_det_modular_given_divisor``.

.. function:: Fmpz_mat_target Fmpz_mat_target::operator=(T:fits_into_slong)
.. function:: Fmpz_mat_target Fmpz_mat_target::operator=(const char*)

C++ particulars
-------------------------------------------------------------------------------

.. function:: slong Fmpz_mat_expr::rows() const
.. function:: slong Fmpz_mat_expr::cols() const

    Obtain the number of rows/columns in this matrix. These functions never
    cause evaluation (the matrix size is computed from the operations in the
    expression template and the size of the input matrices).

.. function:: Fmpz_mat_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``sqr``, ``charpoly``, ``det``,
    ``det_bareiss``, ``det_bound``, ``det_cofactor``,
    ``det_divisor``, ``trace``, ``transpose``.

.. function:: Fmpz_mat_expr::binary operation(??) const

    The following binary functions are made available as member functions:
    ``det_modular``, ``det_modular_accelerated``, ``divexact``,
    ``mul_classical``, ``mul_multi_mod``, ``pow``, 
    code{solve}, ``solve_bound``,
    ``solve_cramer``, ``solve_dixon``, ``solve_fflu``.

Memory management
-------------------------------------------------------------------------------

.. function:: fmpz_matxx::fmpz_matxx(slong i, slong j)

    Allocate a matrix of size $i \times j$.

Basic assignment and manipulation
-------------------------------------------------------------------------------

.. function:: ?? Fmpz_mat_expr::at(T:fits_into_slong, U:fits_into_slong) const

    Unified coefficient access to the matrix entries.

.. function:: void Fmpq_mat_target::set_zero()
.. function:: void Fmpq_mat_target::set_one()
.. function:: static fmpq_matxx fmpq_matxx::zero(slong rows, slong cols)
.. function:: static fmpq_matxx fmpq_matxx::one(slong rows, slong cols)

Input and output
-------------------------------------------------------------------------------

.. function:: print(Fmpz_mat_expr)
.. function:: print(FILE*, Fmpz_mat_expr)
.. function:: print_pretty(Fmpz_mat_expr)
.. function:: print_pretty(FILE*, Fmpz_mat_expr)
.. function:: read(Fmpz_mat_target)
.. function:: read(FILE*, Fmpz_mat_target)

Comparison
-------------------------------------------------------------------------------

The overloaded operator ``==`` can be used for equality testing.
Additionally, we have the following functions.

.. function:: bool Fmpz_mat_expr::is_empty() const
.. function:: bool Fmpz_mat_expr::is_quare() const

Conversion
-------------------------------------------------------------------------------

.. function:: static fmpz_matxx fmpz_matxx::lift(Nmod_mat_expr)
.. function:: static fmpz_matxx fmpz_matxx::lift_unsigned(Nmod_mat_expr)

    See ``fmpz_mat_set_nmod_mat`` and ``fmpz_mat_set_nmod_mat_unsigned``.

.. function:: static fmpz_matxx fmpz_matxx::reduce(Fmpq_mat_expr, Fmz_expr)

    See ``fmpq_mat_get_fmpz_mat_mod_fmpz``.

.. function:: static fmpz_matxx fmpz_matxx::from_integral_fraction(Fmpq_mat_expr)
.. function:: void Fmpz_mat_target::set_integral_fraction(Fmpq_mat_expr)

    See ``fmpq_mat_get_fmpz_mat``. Raises ``flint_exception`` if the
    argument has non-integer entries.

Randomisation
-------------------------------------------------------------------------------

.. function:: void Fmpz_mat_target::set_randbits(frandxx& state, mp_bitcnt_t bits)
.. function:: void Fmpz_mat_target::set_randtest(frandxx& state, mp_bitcnt_t bits)
.. function:: void Fmpz_mat_target::set_randintrel(frandxx& state, mp_bitcnt_t bits)
.. function:: void Fmpz_mat_target::set_randsimdioph(frandxx& state, mp_bitcnt_t bits, mp_bitcount_t bits2)
.. function:: void Fmpz_mat_target::set_randtrulike(frandxx& state, mp_bitcnt_t bits, ulong q)
.. function:: void Fmpz_mat_target::set_randtrulike2(frandxx& state, mp_bitcnt_t bits, ulong q)
.. function:: void Fmpz_mat_target::set_randajtai(frandxx& state, mp_bitcnt_t bits, double alpha)
.. function:: void Fmpz_mat_target::set_randrank(frandxx& state, slong rank, mp_bitcnt_t bits)
.. function:: void Fmpz_mat_target::set_randdet(frandxx& state, Fmpz_expr d)

    See ``fmpz_mat_randbits`` etc.

.. function:: static fmpz_matxx fmpz_matxx::randbits(slong r, slong c, frandxx& state, mp_bitcnt_t bits)
.. function:: static fmpz_matxx fmpz_matxx::randtest(slong r, slong c, frandxx& state, mp_bitcnt_t bits)
.. function:: static fmpz_matxx fmpz_matxx::randintrel(slong r, slong c, frandxx& state, mp_bitcnt_t bits)
.. function:: static fmpz_matxx fmpz_matxx::randsimdioph(slong r, slong c, frandxx& state, mp_bitcnt_t bits, mp_bitcount_t bits2)
.. function:: static fmpz_matxx fmpz_matxx::randtrulike(slong r, slong c, frandxx& state, mp_bitcnt_t bits, ulong q)
.. function:: static fmpz_matxx fmpz_matxx::randtrulike2(slong r, slong c, frandxx& state, mp_bitcnt_t bits, ulong q)
.. function:: static fmpz_matxx fmpz_matxx::randajtai(slong r, slong c, frandxx& state, mp_bitcnt_t bits, double alpha)
.. function:: static fmpz_matxx fmpz_matxx::randrank(slong r, slong c, frandxx& state, slong rank, mp_bitcnt_t bits)
.. function:: static fmpz_matxx fmpz_matxx::randdet(slong r, slong c, frandxx& state, Fmpz_expr d)

    Static versions of the above, where the first two arguments specify the
    dimensions of the matrix.

.. function:: int Fmpz_mat_target::set_randpermdiag(frandxx& state, Vec v)

    See ``fmpz_mat_randpermdiag``. The type ``vec`` must have methods
    ``_array()`` and ``size()`` similar to ``fmpz_vecxx``.

.. function:: void Fmpz_mat_target::apply_randops(frandxx& state, slong count)

    See ``fmpz_mat_randops``.

Transpose
-------------------------------------------------------------------------------

.. function:: Fmpz_expr transpose(Fmpz_mat_expr)

Modular reduction and reconstruction
-------------------------------------------------------------------------------

To reduce a single matrix modulo a word-sized modulus, see
``nmod_matxx::reduce``.

We use a special class ``nmod_mat_vector`` to represent a vector of matrices
reduced with respect to differing moduli.

.. function:: Fmpz_mat_expr CRT(Fmpz_mat_expr, Fmpz_expr, Nmod_mat_expr, bool)

    See ``fmpz_mat_CRT_ui``.

.. function:: nmod_mat_vector::nmod_mat_vector(slong rows, slong cols,
        const std::vector<mp_limb_t>& primes)

    Initialize a vector of matrices with dimensions given by ``rows``,
    ``cols`` and moduli given by ``primes``.

.. function:: nmod_matxx_ref nmod_mat_vector::operator[](std::size_t idx)
.. function:: nmod_matxx_srcref nmod_mat_vector::operator[](std::size_t idx) const

    Obtain a reference to one of the stored matrices.

.. function:: std::size_t nmod_mat_vector::size() const

    Obtain the number of stored matrices.

.. function:: void nmod_mat_vector::set_multi_mod(Fmpz_mat_expr m)

    Reduce ``m`` modulo each of the primes stored in this vector, and store
    the results. See ``fmpz_mat_multi_mod_ui``.

.. function:: void nmod_mat_vector::set_multi_mod_precomp(Fmpz_mat_expr m, const fmpz_combxx& comb)

    Reduce ``m`` modulo each of the primes stored in this vector, and store
    the results. Use precomputed data in ``comp``.
    See ``fmpz_mat_multi_mod_ui_precomp``.

.. function:: nmod_mat_vector multi_mod(Fmpz_mat_expr m, const std::vector<mp_limb_t>& primes)
.. function:: nmod_mat_vector multi_mod_precomp(Fmpz_mat_expr m, const std::vector<mp_limb_t>& primes, const fmpz_combxx& comb)

    Convenience functions combining the allocation of memory and modular
    reduction.

Arithmetic
-------------------------------------------------------------------------------

The overloaded operators ``+ - *`` can be used for ordinary matrix-matrix
and matrix-scalar arithmetic. Additionally, we provide the following functions.

.. function:: Fmpz_mat_expr divexact(Fmpz_mat_expr, Fmpz_expr)
.. function:: Fmpz_mat_expr divexact(Fmpz_mat_expr, T:is_integer)

.. function:: Fmpz_mat_expr mul_classical(Fmpz_mat_expr, Fmpz_mat_expr)
.. function:: Fmpz_mat_expr mul_multi_mod(Fmpz_mat_expr, Fmpz_mat_expr)

.. function:: Fmpz_expr sqr(Fmpz_mat_expr)
.. function:: Fmpz_mat_expr pow(Fmpz_mat_expr, T:is_unsigned_integer)

Inverse
-------------------------------------------------------------------------------

.. function:: Ltuple<bool, fmpz_matxx, fmpzxx>_expr inv(Fmpz_mat_expr)

    ``ltupleref(b, M, D) = inv(A)`` has the same effect as
    ``b = fmpz_mat_inv(m, d, a)``, where ``m, d, a`` are the underlying C
    objects corresponding to ``M, D, A``.

Trace
-------------------------------------------------------------------------------

.. function:: Fmpz_mat_expr trace(Fmpz_mat_expr)

Determinant
-------------------------------------------------------------------------------

.. function:: Fmpz_expr det(Fmpz_mat_expr)
.. function:: Fmpz_expr det_cofactor(Fmpz_mat_expr)
.. function:: Fmpz_expr det_bareiss(Fmpz_mat_expr)
.. function:: Fmpz_expr det_divisor(Fmpz_mat_expr)
.. function:: Fmpz_expr det_bound(Fmpz_mat_expr)
.. function:: Fmpz_expr det_modular(Fmpz_mat_expr, bool proved)
.. function:: Fmpz_expr det_modular_accelerated(Fmpz_mat_expr, bool proved)
.. function:: Fmpz_expr det_modular_given_divisor(Fmpz_mat_expr, Fmpz_expr, bool proved)

Characteristic polynomial
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr charpoly(Fmpz_mat_expr)

Rank
-------------------------------------------------------------------------------

.. function:: slong rank(Fmpz_mat_expr)

Non-singular solving
-------------------------------------------------------------------------------

.. function:: Ltuple<bool, fmpz_matxx, fmpzxx>_expr solve(Fmpz_mat_expr B, Fmpz_mat_expr X)
.. function:: Ltuple<bool, fmpz_matxx, fmpzxx>_expr solve_dixon(Fmpz_mat_expr B, Fmpz_mat_expr X)
.. function:: Ltuple<bool, fmpz_matxx, fmpzxx>_expr solve_cramer(Fmpz_mat_expr B, Fmpz_mat_expr X)
.. function:: Ltuple<bool, fmpz_matxx, fmpzxx>_expr solve_fflu(Fmpz_mat_expr B, Fmpz_mat_expr X)

    ``ltupleref(w, M, D) = solve(B, X)`` has the same effect as
    ``w = fmpz_mat_solve(m, d, b, x)``, where ``m, d, b, x`` are the
    underlying C objects corresponding to ``M, D, B, X``.
    Similarly for the other functions.

.. function::Ltuple<fmpzxx, fmpzxx>_expr solve_bound(Fmpz_mat_expr B, Fmpz_mat_expr X)

Row reduction
-------------------------------------------------------------------------------

Beware that compared to the C interface, the flintxx row reduction interface
changes some argument orders. This is to facilitate default arguments.

.. function:: slong find_pivot_any(Fmpz_mat_expr, slong, slong, slong)

    See ``fmpz_mat_find_pivot_any``.

.. function:: Ltuple<slong, fmpz_matxx, fmpzxx>_expr fflu(Fmpz_mat_expr A, permxx* perm = 0, bool rankcheck = false)

    See ``fmpz_mat_fflu``.

.. function:: Ltuple<slong, fmpz_matxx, fmpzxx>_expr rref(Fmpz_mat_expr A)

    See ``fmpz_mat_rref``.

Modular gaussian elimination
-------------------------------------------------------------------------------

.. function:: slong Fmpz_mat_target::set_rref_mod(Fmpz_expr n, permxx* perm = 0)

    See ``fmpz_mat_rref_mod``.

Nullspace
-------------------------------------------------------------------------------

.. function:: Ltuple<slong, fmpz_matxx>_expr nullspace(Fmpz_mat_expr A)

    ``ltupleref(n, B) = nullspace(A)`` has the same effect as
    ``n = fmpz_mat_nullspace(b, a)``, where ``b, a`` are the underlying
    ``fmpz_mat_t`` corresponding to ``B, A``.


Polynomials over the integers (``fmpz_polyxx``)
===============================================================================

C++ particulars
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``derivative``, ``primitive_part``, ``sqr``, ``sqr_classical``,
    ``sqr_karatsuba``, ``sqr_KS``, ``sqrt``, ``sqrt_classical``,
    ``content``, ``height``, ``bound_roots``, ``twonorm``.

.. function:: Fmpz_poly_expr::binary operation(??) const

    The following binary functions are made available as member functions:
    ``compose_divconquer``, ``compose_horner``, ``div_basecase``,
    ``div_divconquer``, ``divexact``, ``divrem``,
    ``divrem_basecase``, ``divrem_divconquer``, ``div_root``,
    ``evaluate_divconquer``, ``evaluate_horner``, ``fdiv_2exp``,
    ``gcd``, ``gcd_heuristic``, ``gcd_modular``,
    ``gcd_subresultant``, ``inv_series``, ``inv_series_newton``,
    ``lcm``, ``mul_2exp``, ``mul_classical``, ``mul_karatsuba``,
    ``mul_KS``, ``mulmid_classical``, ``mul_SS``,
    ``shift_left``, ``shift_right``, ``pow``,
    ``pow_addchains``, ``pow_binexp``, ``pow_binomial``,
    ``pow_multinomial``, ``pseudo_div``, ``pseudo_divrem``,
    ``pseudo_divrem_basecase``, ``pseudo_divrem_cohen``,
    ``pseudo_divrem_divconquer``, ``pseudo_rem``,
    ``pseudo_rem_cohen``, ``resultant``, ``reverse``,
    ``revert_series``, ``revert_series_lagrange``,
    ``revert_series_lagrange_fast``, ``revert_series_newton``,
    ``smod``, ``sqrlow``, ``sqrlow_classical``,
    ``sqrlow_karatsuba_n``, ``sqrlow_KS``, ``taylor_shift``,
    ``taylor_shift_horner``, ``taylor_shift_divconquer``, ``tdiv``,
    ``tdiv_2exp``, ``xgcd``, ``xgcd_modular``, ``divides``.

.. function:: Fmpz_poly_expr::ternary operation(??, ??) const

    The following ternary functions are made available as member functions:
    ``compose_series``, ``compose_series_brent_kung``,
    ``compose_horner``, ``div_series``, ``mulhigh_classical``,
    ``mulhigh_karatsuba_n``, ``mulhigh_n``, ``mullow``,
    ``mullow_classical``, ``mullow_karatsuba_n``, ``mullow_KS``,
    ``mullow_SS``, ``pow_trunc``.

.. function:: Fmpz_poly_expr Fmpz_poly_expr::operator()(Fmpz_poly_expr) const
.. function:: Fmpz_poly_expr Fmpz_poly_expr::operator()(Fmpz_expr) const

    Overloaded ``operator()`` for evaluation or composition.

Memory management
-------------------------------------------------------------------------------

.. function:: fmpz_polyxx::fmpz_polyxx()

.. function:: fmpz_polyxx::fmpz_polyxx(slong alloc)

    See ``fmpz_poly_init2``.

.. function:: fmpz_polyxx::fmpz_polyxx(const char* str)

    See ``fmpz_poly_set_str``.

.. function:: void Fmpz_poly_target realloc(slong alloc)
.. function:: void Fmpz_poly_target::fit_length(slong len)

.. function:: void Fmpz_poly_target::_normalise()
.. function:: void Fmpz_poly_target::_set_length(slong len)

Polynomial parameters
-------------------------------------------------------------------------------

.. function:: slong Fmpz_poly_expr::length() const
.. function:: slong Fmpz_poly_expr::degree() const

Assignment and basic manipulation
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_target Fmpz_poly_target::operator=(T:is_integer)
.. function:: Fmpz_poly_target Fmpz_poly_target::operator=(Fmpz_expr)
.. function:: Fmpz_poly_target Fmpz_poly_target::operator=(const char*)

.. function:: std::string Fmpz_poly_expr::to_string() const

.. function:: std::string Fmpz_poly_expr::pretty(const char* x) const

    See ``fmpz_poly_get_str_pretty``.

.. function:: void Fmpz_poly_target::set_zero()
.. function:: void Fmpz_poly_target::set_one()
.. function:: static fmpz_polyxx fmpz_polyxx::zero()
.. function:: static fmpz_polyxx fmpz_polyxx::one()

.. function:: void Fmpz_poly_target::zero_coeffs(slong i, slong j)

.. function:: Fmpz_poly_expr reverse(Fmpz_poly_expr, T:fits_into_slong)

.. function:: void Fmpz_poly_target::truncate(slong)

Randomisation
-------------------------------------------------------------------------------

.. function:: static fmpz_polyxx fmpz_polyxx::randtest(frandxx& state, slong len, mp_bitcnt_t bits)
.. function:: static fmpz_polyxx fmpz_polyxx::randtest_unsigned(frandxx& state, slong len, mp_bitcnt_t bits)
.. function:: static fmpz_polyxx fmpz_polyxx::randtest_not_zero(frandxx& state, slong len, mp_bitcnt_t bits)

    See ``fmpz_poly_randtest`` etc.

Getting and setting coefficients
-------------------------------------------------------------------------------

.. function:: Fmpz_expr Fmpz_poly_expr::get_coeff(slong n)

    Obtain coefficient $n$ of the polynomial. It is valid to call this with $n$
    greater than the degree, in which case zero is returned.

.. function:: void Fmpz_poly_target::set_coeff(slong n, Fmpz_expr)
.. function:: void Fmpz_poly_target::set_coeff(slong n, T:is_integer)

.. function:: ?? Fmpz_poly_expr::coeff(slong n) const

    Unified coefficient access for coefficient $n$.
    The result is undefined if $n$ is
    greater than the degree of the polynomial (or negative).

    If the leading coefficient of the polynomial is set to zero in this way, a
    call to ``_normalise`` is necessary.

.. function:: ?? Fmpz_poly_expr::lead() const

    Unified coefficient access for the leading coefficient.
    The result is undefined if
    the length of the polynomial is zero.

    If this is used to set the leading coefficient to zero,
    call to ``_normalise`` is necessary.

Comparison
-------------------------------------------------------------------------------

As usual, ``fmpz_polyxx`` can be compared using ``operator==``.
Additionally, the following functions are provided.

.. function:: bool Fmpz_poly_expr::is_one() const
.. function:: bool Fmpz_poly_expr::is_zero() const
.. function:: bool Fmpz_poly_expr::is_unit() const

Addition and subtraction
-------------------------------------------------------------------------------

The overloaded operators ``+ -`` can be used for addition, subtraction and
negation.

Scalar multiplication and division
-------------------------------------------------------------------------------

The overloaded operators ``* /`` can be used for scalar multiplication and
division, and the operator \code{\%} for remaindering. For finer control, the
following functions are provided.

.. function:: Fmpz_poly_expr mul_2exp(Fmpz_poly_expr, T:is_unsigned_integer)
.. function:: Fmpz_poly_expr fdiv_2exp(Fmpz_poly_expr, T:is_unsigned_integer)

.. function:: Fmpz_poly_expr tdiv(Fmpz_poly_expr, Fmpz_expr)
.. function:: Fmpz_poly_expr tdiv(Fmpz_poly_expr, T:is_integer)
.. function:: Fmpz_poly_expr divexact(Fmpz_poly_expr, Fmpz_expr)
.. function:: Fmpz_poly_expr divexact(Fmpz_poly_expr, T:is_integer)

.. function:: Fmpz_poly_expr smod(Fmpz_poly_expr, Fmpz_expr)

    See ``fmpz_poly_scalar_smod_fmpz``.

Bit packing
-------------------------------------------------------------------------------

.. function:: Fmpz_expr bit_pack(Fmpz_poly_expr, T:fits_into_mp_bitcnt_t)
.. function:: static Fmpz_poly_expr fmpz_polyxx::bit_unpack(Fmpz_expr, T:fits_into_mp_bitcnt_t)
.. function:: static Fmpz_poly_expr fmpz_polyxx::bit_unpack_unsigned(Fmpz_expr, traits::fits_into_mp_bitcnt_t)

Multiplication
-------------------------------------------------------------------------------

The overloaded operator ``*`` can also be used for poly-poly multiplication.
Additionally, the following functions are provided.

.. function:: Fmpz_poly_expr mul_classical(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr mulmid_classical(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr mul_karatsuba(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr mul_SS(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr mul_KS(Fmpz_poly_expr, Fmpz_poly_expr)

.. function:: Fmpz_poly_expr mullow(Fmpz_poly_expr, Fmpz_poly_expr, slong)
.. function:: Fmpz_poly_expr mullow_classical(Fmpz_poly_expr, Fmpz_poly_expr, slong)
.. function:: Fmpz_poly_expr mullow_karatsuba_n(Fmpz_poly_expr, Fmpz_poly_expr, slong)
.. function:: Fmpz_poly_expr mullow_KS(Fmpz_poly_expr, Fmpz_poly_expr, slong)
.. function:: Fmpz_poly_expr mullow_SS(Fmpz_poly_expr, Fmpz_poly_expr, slong)

.. function:: Fmpz_poly_expr mulhigh_n(Fmpz_poly_expr, Fmpz_poly_expr, slong)
.. function:: Fmpz_poly_expr mulhigh_classical(Fmpz_poly_expr, Fmpz_poly_expr, slong)
.. function:: Fmpz_poly_expr mulhigh_karatsuba_n(Fmpz_poly_expr, Fmpz_poly_expr, slong)

Squaring
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr sqr(Fmpz_poly_expr)
.. function:: Fmpz_poly_expr sqr_KS(Fmpz_poly_expr)
.. function:: Fmpz_poly_expr sqr_karatsuba(Fmpz_poly_expr)
.. function:: Fmpz_poly_expr sqr_classical(Fmpz_poly_expr)

.. function:: Fmpz_poly_expr sqrlow(Fmpz_poly_expr, T:fits_into_slong n)
.. function:: Fmpz_poly_expr sqrlow_classical(Fmpz_poly_expr, T:fits_into_slong n)
.. function:: Fmpz_poly_expr sqrlow_KS(Fmpz_poly_expr, T:fits_into_slong n)
.. function:: Fmpz_poly_expr sqrlow_karatsuba_n(Fmpz_poly_expr, T:fits_into_slong n)

Powering
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr pow(Fmpz_poly_expr, T:is_unsigned_integer)
.. function:: Fmpz_poly_expr pow_multinomial(Fmpz_poly_expr, T:is_unsigned_integer)
.. function:: Fmpz_poly_expr pow_binomial(Fmpz_poly_expr, T:is_unsigned_integer)
.. function:: Fmpz_poly_expr pow_binexp(Fmpz_poly_expr, T:is_unsigned_integer)
.. function:: Fmpz_poly_expr pow_addchains(Fmpz_poly_expr, T:is_unsigned_integer)
.. function:: Fmpz_poly_expr pow_trunc(Fmpz_poly_expr, ulong e, slong n)

Shifting
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr shift_left(Fmpz_poly_expr, T:fits_into_slong)
.. function:: Fmpz_poly_expr shift_right(Fmpz_poly_expr, T:fits_into_slong)

Bit sizes and norms
-------------------------------------------------------------------------------

.. function:: Fmpz_expr height(Fmpz_poly_expr)
.. function:: Fmpz_expr twonorm(Fmpz_poly_expr)

.. function:: ulong Fmpz_poly_expr::max_limbs() const
.. function:: slong Fmpz_poly_expr::max_bits() const

Greatest common divisor
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr gcd(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr gcd_subresultant(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr gcd_heuristic(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr gcd_modular(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr lcm(Fmpz_poly_expr, Fmpz_poly_expr)

.. function:: Ltuple<fmpzxx, fmpz_polyxx, fmpz_polyxx>_expr xgcd(Fmpz_poly_expr f, Fmpz_poly_expr g)
.. function:: Ltuple<fmpzxx, fmpz_polyxx, fmpz_polyxx>_expr xgcd_modular(Fmpz_poly_expr f, Fmpz_poly_expr g)

    ``ltupleref(N, Q, R) = xgcd(F, G)`` has the same effect as
    ``fmpz_poly_xgcd(n, q, r, f, g)`` where ``n, q, r, f, g`` are the
    underlying C objects.

.. function:: Fmpz_expr resultant(Fmpz_poly_expr)

Gaussian content
-------------------------------------------------------------------------------

.. function:: Fmpz_expr content(Fmpz_poly_expr)
.. function:: Fmpz_poly_expr primitive_part(Fmpz_poly_expr)

Square-free
-------------------------------------------------------------------------------

.. function:: bool Fmpz_poly_expr::is_squarefree() const

Euclidean division
-------------------------------------------------------------------------------

The overloaded operators ``/ %`` can be used for euclidean division and
remainder. Additionally, the following functions are provided.

.. function:: Fmpz_poly_expr div_basecase(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr div_divconquer(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr rem_basecase(Fmpz_poly_expr, Fmpz_poly_expr)

.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx>_expr divrem(Fmpz_poly_expr A, Fmpz_poly_expr B)
.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx>_expr divrem_basecase(Fmpz_poly_expr A, Fmpz_poly_expr B)
.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx>_expr divrem_divconquer(Fmpz_poly_expr A, Fmpz_poly_expr B)

    ``ltupleref(Q, R) = divrem(A, B)`` has the same effect as
    ``fmpz_poly_divrem(q, r, a, b)``, where ``q, r, a, b`` are the
    underlying ``fmpz_poly_t`` corresponding to ``Q, R, A, B``.

.. function:: Fmpz_poly_expr div_root(Fmpz_poly_expr, Fmpz_expr)

Divisibility testing
-------------------------------------------------------------------------------

.. function:: Ltuple<bool, fmpz_polyxx>_expr divides(Fmpz_poly_expr A, Fmpz_poly_expr B)

    ``ltupleref(d, Q) = divides(A, B)`` sets ``d`` to ``true``
    and ``Q`` to ``A/B`` if ``B`` divides ``A``, and else sets
    ``d`` to ``false``. See ``fmpz_poly_divides``.

Power series division
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr inv_series_newton(Fmpz_poly_expr, T:fits_into_slong)
.. function:: Fmpz_poly_expr inv_series(Fmpz_poly_expr, T:fits_into_slong)
.. function:: Fmpz_poly_expr div_series(Fmpz_poly_expr, Fmpz_poly_expr, slong n)

Pseudo division
-------------------------------------------------------------------------------

.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx, ulong>_expr pseudo_divrem(Fmpz_poly_expr A, Fmpz_poly_expr B)
.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx, ulong>_expr pseudo_divrem_basecase(Fmpz_poly_expr A, Fmpz_poly_expr B)
.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx, ulong>_expr pseudo_divrem_divconquer(Fmpz_poly_expr A, Fmpz_poly_expr B)

    ``ltupleref(Q, R, d) = pseudo_divrem(A, B)`` has the same effect as\\
    ``fmpz_poly_pseudo_divrem(q, r, &d, a, b)``, where ``q, r, a, b`` are
    the underlying ``fmpz_poly_t`` corresponding to ``Q, R, A, B``.

.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx>_expr pseudo_divrem_cohen(Fmpz_poly_expr A, Fmpz_poly_expr B)

    ``ltupleref(Q, R) = pseudo_divrem_cohen(A, B)`` has the same effect as\\
    ``fmpz_poly_pseudo_divrem_cohen(q, r, a, b)``, where ``q, r, a, b``
    are the underlying ``fmpz_poly_t`` corresponding to ``Q, R, A, B``.

.. function:: Ltuple<fmpz_polyxx, ulong>_expr pseudo_div(Fmpz_poly_expr A, Fmpz_poly_expr B)
.. function:: Ltuple<fmpz_polyxx, ulong>_expr pseudo_rem(Fmpz_poly_expr A, Fmpz_poly_expr B)

    ``ltupleref(Q, d) = pseudo_div(A, B)`` has the same effect as
    ``fmpz_poly_pseudo_div(q, &d, a, b)``, where ``q, a, b`` are the
    underlying ``fmpz_poly_t`` corresponding to ``Q, A, B``.

.. function:: Fmpz_poly_expr pseudorem_cohen(Fmpz_poly_expr, Fmpz_poly_expr)

Derivative
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr derivative(Fmpz_poly_expr)

Evaluation
-------------------------------------------------------------------------------

The overloaded ``operator()`` can be used for evaluation. Additionally, the
following functions are provided.

.. function:: Fmpz_expr evaluate(Fmpz_poly_expr, Fmpz_expr)
.. function:: Fmpz_vec_expr evaluate(Fmpz_poly_expr, Fmpz_vec_expr)
.. function:: Fmpz_expr evaluate_horner(Fmpz_poly_expr, Fmpz_expr)
.. function:: Fmpz_expr evaluate_divconquer(Fmpz_poly_expr, Fmpz_expr)

.. function:: mp_limb_t evaluate_mod(Fmpz_poly_expr p, mp_limb_t x, mp_limb_t n)

Interpolation
-------------------------------------------------------------------------------

.. function:: static Fmpz_poly_expr fmpz_polyxx::interpolate(Fmpz_vec_expr xs, Fmpz_vec_expr ys)

    See ``fmpz_poly_interpolate_fmpz_vec``.

Composition.
-------------------------------------------------------------------------------

The overloaded ``operator()`` can be used for composition. Additionally, the
following functions are provided.

.. function:: Fmpz_poly_expr compose(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr compose_horner(Fmpz_poly_expr, Fmpz_poly_expr)
.. function:: Fmpz_poly_expr compose_divconquer(Fmpz_poly_expr, Fmpz_poly_expr)

Taylor shift
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr taylor_shift(Fmpz_poly_expr, Fmpz_expr)
.. function:: Fmpz_poly_expr taylor_shift_horner(Fmpz_poly_expr, Fmpz_expr)
.. function:: Fmpz_poly_expr taylor_shift_divconquer(Fmpz_poly_expr, Fmpz_expr)

Power series composition
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr compose_series(Fmpz_poly_expr, Fmpz_poly_expr, slong)
.. function:: Fmpz_poly_expr compose_series_horner(Fmpz_poly_expr, Fmpz_poly_expr, slong)
.. function:: Fmpz_poly_expr compose_series_brent_kung(Fmpz_poly_expr, Fmpz_poly_expr, slong)

Power series reversion
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr revert_series(Fmpz_poly_expr, T:fits_into_slong)
.. function:: Fmpz_poly_expr revert_series_newton(Fmpz_poly_expr, T:fits_into_slong)
.. function:: Fmpz_poly_expr revert_series_lagrange(Fmpz_poly_expr, T:fits_into_slong)
.. function:: Fmpz_poly_expr revert_series_lagrange_fast(Fmpz_poly_expr, T:fits_into_slong)

Square root
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr sqrt(Fmpz_poly_expr p)
.. function:: Fmpz_poly_expr sqrt_classical(Fmpz_poly_expr p)

    Compute the square root of ``p``, provided ``p`` is a perfect square.
    Else raise ``flint_exception``. See ``fmpz_poly_sqrt``.

Signature
-------------------------------------------------------------------------------

.. function:: void Fmpz_poly_expr::signature(slong& r1, slong& r2) const

    See ``fmpz_poly_signature``.

Hensel lifting
-------------------------------------------------------------------------------

.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx, fmpz_polyxx, fmpz_polyxx>_expr hensel_lift(Fmpz_poly_expr f, Fmpz_poly_expr g, Fmpz_poly_expr h, Fmpz_poly_expr a, Fmpz_poly_expr b, Fmpz_expr p, Fmpz_expr p1)
.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx>_expr hensel_lift_without_inverse(Fmpz_poly_expr f, Fmpz_poly_expr g, Fmpz_poly_expr h, Fmpz_poly_expr a, Fmpz_poly_expr b, Fmpz_expr p, Fmpz_expr p1)
.. function:: Ltuple<fmpz_polyxx, fmpz_polyxx>_expr hensel_lift_only_inverse(Fmpz_poly_expr G, Fmpz_poly_expr H, Fmpz_poly_expr a, Fmpz_poly_expr b, Fmpz_expr p, Fmpz_expr p1)

    See ``fmpz_poly_hensel_lift`` etc.

.. function:: fmpz_poly_factorxx::set_hensel_lift_once(Fmpz_poly_expr, const nmod_poly_factorxx&, slong)
.. function:: fmpz_poly_factorxx hensel_lift_once(Fmpz_poly_expr, const nmod_poly_factorxx&, slong)

    See ``fmpz_poly_hensel_lift_once``. Note that these two
    functions are defined in the ``fmpz_factorxx`` module.

Input and output
-------------------------------------------------------------------------------

.. function:: print(Fmpz_poly_expr)
.. function:: print(FILE*, Fmpz_poly_expr)
.. function:: print_pretty(Fmpz_poly_expr, const char* var)
.. function:: print_pretty(FILE*, Fmpz_poly_expr, const char* var)
.. function:: read(Fmpz_poly_target)
.. function:: read(FILE*, Fmpz_poly_target)
.. function:: read_pretty(Fmpz_poly_target, const char* var)
.. function:: read_pretty(FILE*, Fmpz_poly_target, const char* var)

Modular reduction and reconstruction
-------------------------------------------------------------------------------

For modular reduction, see ``nmod_polyxx::reduce``.

.. function:: Fmpz_poly_expr Fmpz_poly_expr::CRT(Fmpz_expr, Nmod_poly_expr, bool)
.. function:: Fmpz_poly_expr CRT(Fmpz_poly_expr, Fmpz_expr, Nmod_poly_expr, bool)

    See ``fmpz_poly_CRT_ui``.

Products
-------------------------------------------------------------------------------

.. function:: static Fmpz_poly_expr fmpz_polyxx::product_roots(Fmpz_vec_expr xs)

    See ``fmpz_poly_product_roots_fmpz_vec``.

Roots
-------------------------------------------------------------------------------

.. function:: Fmpz_expr bound_roots(Fmpz_poly_expr p)


Factorisation of polynomials over the integers (``fmpz_poly_factorxx``)
===============================================================================

Miscellaneous
-------------------------------------------------------------------------------

.. function:: bool fmpz_poly_factorxx::operator==(const fmpz_poly_factorxx&)

    Compare two factorisations.

.. function:: ulong fmpz_poly_factorxx::size() const

    Return the number of stored factors.

.. function:: slong fmpz_poly_factorxx::exp(slong i) const
.. function:: slong& fmpz_poly_factorxx::exp(slong i)

    Obtain the exponent of the ith factor.

.. function:: fmpz_polyxx_srcref fmpz_poly_factorxx::p(slong i) const
.. function:: fmpz_polyxx_ref fmpz_poly_factorxx::p(slong i)

    Obtain the ith factor.

.. function:: fmpzxx_srcref fmpz_poly_factorxx::content() const
.. function:: fmpzxx_ref fmpz_poly_factorxx::content()

    Obtain the content of the factorised polynomial.

Memory management
-------------------------------------------------------------------------------

.. function:: fmpz_poly_factorxx::fmpz_poly_factorxx()
.. function:: explicit fmpz_poly_factorxx::fmpz_poly_factorxx(slong alloc)

    Initialise an empty factorisation.

.. function:: fmpz_poly_factorxx::fmpz_poly_factorxx(const fmpz_poly_factorxx& o)

    Copy a factorisation.

.. function:: void fmpz_poly_factorxx::realloc(slong a)
.. function:: void fmpz_poly_factorxx::fit_length(slong a)

Manipulating factors
-------------------------------------------------------------------------------

.. function:: void fmpz_poly_factorxx::insert(Fmpz_poly_expr p, slong e)
.. function:: void fmpz_poly_factorxx::concat(const fmpz_poly_factorxx&)

Factoring algorithms
-------------------------------------------------------------------------------

.. function:: void fmpz_poly_factorxx::set_factor_squarefree(Fmpz_poly_expr p)
.. function:: void fmpz_poly_factorxx::set_factor_zassenhaus(Fmpz_poly_expr p)
.. function:: void fmpz_poly_factorxx::set_factor_zassenhaus_recombination(const fmpz_poly_factorxx& lifted_fac, Fmpz_poly_expr F, Fmpz_expr P, slong exp)

.. function:: fmpz_poly_factorxx::factor_squarefree(Fmpz_poly_expr)
.. function:: fmpz_poly_factorxx::factor_zassenhaus(Fmpz_poly_expr)



Rationals (``fmpq``)
===============================================================================

C++ particulars
-------------------------------------------------------------------------------

.. function:: ?? Fmpq_expr::num() const
.. function:: ?? Fmpq_expr::den() const

    Unified coefficient access to numerator and denominator. If this is used to
    modify the object, a call to ``canonicalise()`` may be necessary.

Memory management
-------------------------------------------------------------------------------

.. function:: fmpqxx::fmqxx()

    Initialize to zero.

.. function:: fmpqxx::fmpqxx(Fmpz_src num, Fmpz_src den)
.. function:: fmpqxx::fmpqxx(T:fits_into_slong num, U:is_unsigned_integer den)

    Initialize from numerator ``num`` and denominator ``den``.

Canonicalisation
-------------------------------------------------------------------------------

.. function:: void Fmpq_target::canonicalise()
.. function:: bool Fmpq_src::is_canonical() const

Basic assignment
-------------------------------------------------------------------------------

.. function:: Fmpq_expr Fmpq_expr::abs() const
.. function:: Fmpq_expr abs(Fmpq_expr)

.. function:: void Fmpq_target::set_zero()
.. function:: void Fmpq_target::set_one()
.. function:: static fmpqxx fmpqxx::zero()
.. function:: static fmpqxx fmpqxx::one()

Comparison
-------------------------------------------------------------------------------

The overloaded relational operators can be used for comparison. Additionally,
we have the following functions.

.. function:: bool Fmpq_expr::is_zero() const
.. function:: bool Fmpq_expr::is_one() const
.. function:: int Fmpq_expr::sgn() const
.. function:: mp_bitcnt_t Fmpq_expr::height_bits() const
.. function:: Fmpz_expr Fmpq_expr::height() const

.. function:: mp_bitcnt_t height_bits(Fmpq_expr)
.. function:: Fmpq_expr height(Fmpq_expr)

Conversion
-------------------------------------------------------------------------------

Conversion can be done using the assignment operator, and through the
following functions.

.. function:: static fmpqxx fmpqxx::frac(const T& t, const U& u)

    Same as ``fmpqxx res;res.set_frac(t, u)``.

.. function:: static fmpqxx fmpqxx::integer(const T& t)

    Same as ``fmpqxx res;res.set_integer(t)``.

.. function:: void Fmpq_target::set_frac(const T& t, const U& u)

    ``f.set_frac(t, u)`` has the same effect as
    ``f.num() = t;f.den() = u;f.canonicalise()``.

.. function:: void Fmpq_target::set_integer(const T& t)

    ``f.set_integer(t)`` has the same effect as
    ``f.num() = t;f.den() = 1u``;

.. function:: std::string Fmpq_expr::to_string(int base = 10) const

Input and output
-------------------------------------------------------------------------------

.. function:: int print(Fmpq_expr)
.. function:: int print(FILE*, Fmpq_expr)

Random number generation
-------------------------------------------------------------------------------

.. function:: static fmpqxx fmpqxx::randbits(frandxx& state)

.. function:: static fmpqxx fmpqxx::randtest(frandxx& state)

.. function:: static fmpqxx fmpqxx::randtest_not_zero(frandxx& state)

Arithmetic
-------------------------------------------------------------------------------

The overloaded operators ``+ - * /`` can be used for arithmetic.
Additionally, we provide the following functions.

.. function:: Fmpq_expr Fmpq_expr::inv() const
.. function:: Fmpq_expr Fmpq_expr::pow(T:fits_into_slong) const
.. function:: Fmpq_expr inv(Fmpq_expr)
.. function:: Fmpq_expr pow(Fmpq_expr, T:fits_into_slong)

.. function:: Fmpq_expr operator<<(Fmpq_expr, T:is_integer)
.. function:: Fmpq_expr operator>>(Fmpq_expr, T:is_integer)

    Shift operators are overloaded. See ``fmpq_div_2exp`` and
    ``fmpq_mul_2exp``.

Modular reduction and rational reconstruction
-------------------------------------------------------------------------------

.. function:: Fmpq_expr operator%(Fmpq_expr, Fmpz_expr)

    See ``fmpq_mod_fmpz``.
    The modular reduction operator may raise a ``flint_exception`` if
    modular inversion is not possible.

.. function:: static Fmpq_expr fmpqxx::reconstruct(Fmpz_expr a, Fmpz_expr m, Fmpz_expr N, Fmpz_expr D)
.. function:: static Fmpq_expr fmpqxx::reconstruct(Fmpz_expr a, Fmpz_expr m)

    Rational reconstruction. May raise a ``flint_exception`` if
    reconstruction is not possible. See ``fmpq_reconstruct_fmpz`` and
    ``fmpq_reconstruct_fmpz2``.

Rational enumeration
-------------------------------------------------------------------------------

.. function:: Fmpq_expr Fmpq_expr::next_minimal() const
.. function:: Fmpq_expr Fmpq_expr::next_signed_minimal() const
.. function:: Fmpq_expr Fmpq_expr::next_calkin_wilf() const
.. function:: Fmpq_expr Fmpq_expr::next_signed_calkin_wilf() const

Continued fractions
-------------------------------------------------------------------------------

.. function:: slong Fmpq_expr::cfrac_bound() const

.. function:: template<class Vec> void Fmpq_target::set_cfrac(const Vec& v, slong n)

    Set value to a partial fraction expansion. The same conventions apply to
    ``v`` as in the constructor.

.. function:: template<class Vec> static fmpqxx fmpqxx::from_cfrac(const Vec& v, slong n)

    Initialize from a partial fraction expansion. ``v`` must be an instance
    of a class which provides a method ``_array()`` that returns (a
    pointer to) an array of ``fmpz``. One such class is ``fmpz_vecxx``.
    The array must have size (at least) ``n``.

Matrices over the rationals (``fmpq_matxx``)
===============================================================================

The class ``fmpq_matxx`` wraps ``fmpq_mat_t``. Like ``fmpz_matxx``,
many operations on ``fmpq_matxx`` do not support aliasing. The details can
be found in the documentation of ``fmpq_mat_t``. Since ``fmpq_matxx``
does not use temporary merging, evaluation of subexpressions never creates new
aliases.

.. function:: Fmpq_mat_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``inv``, ``transpose``, ``det``, ``trace``,
    ``numden_entrywise``, ``numden_rowwise``, ``numden_colwise``,
    ``numden_matwise``, ``num_rowwise``.

.. function:: Fmpq_mat_expr::binary operation(??) const

    The following binary functions are made available as member functions:
    ``solve_dixon``, ``solve_fraction_free``, ``mul_cleared``,
    ``mul_direct``.

.. function:: Fmpq_mat_expr operator?(??, ??)

    Arithmetic operators ``+ - * /`` are overloaded when provided by
    ``fmpq_mat_t``.

.. function:: Fmpq_mat_expr operator-(Fmpq_mat_expr)

    The unary negation operator is overloaded.

.. function:: Fmpq_mat_target Fmpq_mat_target::operator=(Fmpz_mat_expr)

    See ``fmpq_mat_set_fmpz_mat``.

Memory management
-------------------------------------------------------------------------------

.. function:: fmpq_matxx::fmpq_matxx(slong m, slong n)

    See ``fmpq_mat_init``.

Input and output
-------------------------------------------------------------------------------

.. function:: int print(Fmpq_mat_expr)

Entry access
-------------------------------------------------------------------------------

.. function:: ?? Fmpq_mat_expr::at(slong, slong) const

    Unified coefficient access to the entries of the matrix.

Basic assignment
-------------------------------------------------------------------------------

.. function:: Fmpq_mat_expr transpose(Fmpq_poly_mat_expr)

.. function:: void Fmpq_mat_target::set_zero()
.. function:: void Fmpq_mat_target::set_one()
.. function:: static fmpq_matxx fmpq_matxx::zero(slong rows, slong cols)
.. function:: static fmpq_matxx fmpq_matxx::one(slong rows, slong cols)

Random matrix generation
-------------------------------------------------------------------------------

.. function:: void Fmpq_mat_target::set_randtest(frandxx& state, slong len, mp_bitcnt_t)
.. function:: static fmpq_matxx fmpq_matxx::randtest(slong rows, slong cols, frandxx& state, slong len, mp_bitcnt_t)
.. function:: void Fmpq_mat_target::set_randtest_unsigned(frandxx& state, slong len, mp_bitcnt_t)
.. function:: static fmpq_matxx fmpq_matxx::randtest_unsigned(slong rows, slong cols, frandxx& state, slong len, mp_bitcnt_t)

Special matrices
-------------------------------------------------------------------------------

.. function:: void Fmpq_target::set_hilbert_matrix()

.. function:: Fmpq_mat_expr hilbert_matrix(slong m, slong n)

Basic properties
-------------------------------------------------------------------------------

.. function:: bool Fmpq_mat_expr::is_zero() const
.. function:: bool Fmpq_mat_expr::is_empty() const
.. function:: bool Fmpq_mat_expr::is_square() const
.. function:: bool Fmpq_mat_expr::is_integral() const

Integer matrix conversion
-------------------------------------------------------------------------------

.. function:: static fmpq_matxx fmpq_matxx::frac(Fmpz_mat_expr, Fmpz_expr)
.. function:: void Fmpq_mat_target::set_frac(Fmpz_mat_expr, Fmpz_expr)

    See ``fmpq_mat_set_fmpz_mat_div_fmpz``.

.. function:: static fmpq_matxx fmpq_matxx::integer_matrix(Fmpz_mat_expr)

    See ``fmpq_mat_set_fmpz_mat``.

.. function:: Fmpz_mat_expr num_rowwise(Fmpq_mat_expr)

    This has the effect of calling ``fmpq_mat_get_fmpz_mat_rowwise`` with
    second argument ``NULL``.

.. function:: Ltuple<fmpz_matxx, fmpz_matxx>_expr numden_entrywise(Fmpq_mat_expr)

    See ``fmpq_mat_get_fmpz_mat_entrywise``.

.. function:: Ltuple<fmpz_matxx, fmpzxx>_expr numden_matwise(Fmpq_mat_expr)

    See ``fmpq_mat_get_fmpz_mat_matwise``.

.. function:: Ltuple<fmpz_matxx, fmpz_vecxx>_expr numden_rowwise(Fmpq_mat_expr)

    See ``fmpq_mat_get_fmpz_mat_rowwise``.

.. function:: Ltuple<fmpz_matxx, fmpz_vecxx>_expr numden_colwise(Fmpq_mat_expr)

    See ``fmpq_mat_get_fmpz_mat_colwise``.

Modular reduction and rational reconstruction
-------------------------------------------------------------------------------

To reduce an ``fmpq_matxx`` modulo an ``fmpzxx`` to get an
``fmpz_matxx``, see ``fmpz_matxx::reduce``.

.. function:: static fmpq_matxx fmpq_matxx::reconstruct(Fmpz_mat_expr, Fmpz_expr)

    See ``fmpq_mat_set_fmpz_mat_mod_fmpz``.

Matrix multiplication
-------------------------------------------------------------------------------

The overloaded ``operator*`` can be used for matrix multiplication. Finer
control can be obtained using the following functions.

.. function:: Fmpq_mat_expr mul_direct(Fmpq_mat_expr, Fmpq_mat_expr)
.. function:: Fmpq_mat_expr mul_cleared(Fmpq_mat_expr, Fmpq_mat_expr)

Trace
-------------------------------------------------------------------------------

.. function:: Fmpq_expr trace(Fmpq_mat_expr)

Determinant
-------------------------------------------------------------------------------

.. function:: Fmpq_expr det(Fmpq_mat_expr)

Nonsingular solving
-------------------------------------------------------------------------------

.. function:: Fmpq_mat_expr solve_dixon(Fmpq_mat_expr B, Fmpq_mat_expr X)
.. function:: Fmpq_mat_expr solve_fraction_free(Fmpq_mat_expr B, Fmpq_mat_expr X)

    See ``fmpq_mat_solve_dixon`` and ``fmpq_mat_solve_fraction_free``.
    Raises ``flint_exception`` if $B$ is singular.

Inverse
-------------------------------------------------------------------------------

.. function:: Fmpq_mat_expr inv(Fmpq_mat_expr A)

    Compute the inverse of the square matrix $A$. Raises ``flint_exception``
    if $A$ is singular. The modulus is required to be prime.

Echelon form
-------------------------------------------------------------------------------

.. function:: bool Fmpq_mat_target::pivot(slong r, slong c, permxx* perm = 0)

    See ``fmpq_mat_pivot``.

.. function:: Ltuple<slong, fmpq_matxx>_expr rref(Fmpq_mat_expr)
.. function:: Ltuple<slong, fmpq_matxx>_expr rref_classical(Fmpq_mat_expr)
.. function:: Ltuple<slong, fmpq_matxx>_expr rref_fraction_free(Fmpq_mat_expr)

    See ``fmpq_mat_rref`` etc.

Polynomials over the rationals (``fmpq_poly``)
===============================================================================

C++ particulars
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr Fmpq_poly_expr::operator()(Fmpq_poly_expr) const
.. function:: Fmpq_poly_expr Fmpq_poly_expr::operator()(Fmpq_expr) const

    Overloaded ``operator()`` for evaluation or composition.

.. function:: Fmpq_poly_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``derivative``, ``integral``, ``inv``, ``make_monic``,
    ``primitive_part``, ``content``.

.. function:: Fmpq_poly_expr::binary operation(??) const

    The following binary functions are made available as member functions:
    ``asinh_series``, ``asin_series``, ``atanh_series``,
    ``atan_series``, ``cosh_series``, ``cos_series``, ``divrem``,
    ``exp_series``, ``gcd``, ``inv_series``, ``inv_series_newton``,
    ``lcm``, ``log_series``, ``pow``, ``resultant``,
    ``reverse``, ``revert_series``, ``revert_series_lagrange``,
    ``revert_series_lagrange_fast``, ``revert_series_newton``,
    ``sinh_series``, ``tanh_series``, ``tan_series``, ``xgcd``,
    ``rescale``,\\
    ``shift_left``, ``shift_right``.

.. function:: Fmpq_poly_expr::ternary operation(??, ??) const

    The following ternary functions are made available as member functions:
    ``compose_series``, ``compose_series_brent_kung``,
    ``compose_series_horner``, ``div_series``, ``mullow``.

Memory management
-------------------------------------------------------------------------------

.. function:: fmpq_polyxx::fmpq_polyxx()

.. function:: fmpq_polyxx::fmpq_polyxx(slong alloc)

    See ``fmpq_poly_init2``.

.. function:: fmpq_polyxx::fmpq_polyxx(const char* str)

    See ``fmpq_poly_set_str``.

.. function:: void Fmpq_poly_target realloc(slong alloc)
.. function:: void Fmpq_poly_target::fit_length(slong len)
.. function:: void Fmpq_poly_target::_normalise()
.. function:: void Fmpq_poly_target::_set_length(slong len)
.. function:: void Fmpq_poly_target::canonicalise()
.. function:: bool Fmpq_poly_src::is_canonical() const

Polynomial parameters
-------------------------------------------------------------------------------

.. function:: slong Fmpq_poly_expr::length() const
.. function:: slong Fmpq_poly_expr::degree() const

Accessing the numerator and denominator
-------------------------------------------------------------------------------

.. function:: fmpqxx_ref Fmpq_poly_target::get_coeff_numref(slong n)
.. function:: fmpqxx_srcref Fmpq_poly_src::get_coeff_numref(slong n) const

    Obtain a reference to the numerator of coefficient $n$.
    The result is undefined if $n$ is
    greater than the degree of the polynomial (or negative).
    If this is used to modify the object, a call to ``canonicalise()`` may
    be necessary.
    (No unified access, see ``get_coeff``.)

.. function:: ?? Fmpq_poly_expr::den() const

    Unified coefficient access to the denominator of the polynomial.
    If this is used to modify the object, a call to ``canonicalise()`` may
    be necessary.

Random testing
-------------------------------------------------------------------------------

.. function:: static fmpq_polyxx fmpq_polyxx::randtest(frandxx& state, slong len, mp_bitcnt_t bits)
.. function:: static fmpq_polyxx fmpq_polyxx::randtest_unsigned(frandxx& state, slong len, mp_bitcnt_t bits)
.. function:: static fmpq_polyxx fmpq_polyxx::randtest_not_zero(frandxx& state, slong len, mp_bitcnt_t bits)

    See ``fmpq_poly_randtest`` etc.

Assignment
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_target Fmpq_poly_target::operator=(T:is_integer)
.. function:: Fmpq_poly_target Fmpq_poly_target::operator=(Fmpq_expr)
.. function:: Fmpq_poly_target Fmpq_poly_target::operator=(Fmpz_expr)
.. function:: Fmpq_poly_target Fmpq_poly_target::operator=(Fmpz_poly_expr)
.. function:: Fmpq_poly_target Fmpq_poly_target::operator=(const char*)

.. function:: void Fmpq_poly_target::set_zero()
.. function:: void Fmpq_poly_target::set_one()
.. function:: static fmpq_polyxx fmpq_polyxx::zero()
.. function:: static fmpq_polyxx fmpq_polyxx::one()

.. function:: Fmpq_poly_expr inv(Fmpq_poly_expr)

.. function:: static fmpq_polyxx fmpq_polyxx::get_slice(Fmpq_poly_expr, slong i, slong j)
.. function:: void Fmpq_poly_target::truncate(slong)

.. function:: Fmpq_poly_expr reverse(Fmpq_poly_expr, T:fits_into_slong)

.. function:: std::string Fmpq_poly_expr::pretty(const char* x) const

    See ``fmpq_poly_get_str_pretty``.

.. function:: std::string Fmpq_poly_expr::to_string() const

Getting and setting coefficients
-------------------------------------------------------------------------------

.. function:: Fmpqxx_expr Fmpq_poly_expr::get_coeff(slong n) const

.. function:: void Fmpq_poly_target::set_coeff(slong n, Fmpz_expr)
.. function:: void Fmpq_poly_target::set_coeff(slong n, Fmpq_expr)
.. function:: void Fmpq_poly_target::set_coeff(slong n, T:is_integer)

Comparison
-------------------------------------------------------------------------------

The overloaded operators ``== != >= >`` etc. can be used for comparison.
Additionally, we have the following functions.

.. function:: bool Fmpq_poly_expr::is_one() const
.. function:: bool Fmpq_poly_expr::is_zero() const

Arithmetic
-------------------------------------------------------------------------------

The overloaded operators ``* / + -`` can be used for both
polynomial-polynomial and polynomial-scalar arithmetic. Additionally, we
provide the following functions.

.. function:: Fmpq_poly_expr mullow(Fmpq_poly_expr, Fmpq_poly_expr, slong)

Powering
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr pow(Fmpq_poly_expr, T:is_unsigned_integer)

Shifting
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr shift_left(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr shift_right(Fmpq_poly_expr, T:fits_into_slong)

Euclidean division
-------------------------------------------------------------------------------

The overloaded operators ``/ %`` can be used for euclidean division and
remainder. Additionally, we have the following functions.

.. function:: Ltuple<fmpq_polyxx, fmpq_polyxx>_expr divrem(Fmpq_poly_expr A, Fmpq_poly_expr B)

    ``ltupleref(Q, R) = divrem(A, B)`` has the same effect as
    ``fmpq_poly_divrem(q, r, a, b)`` where ``q, r, a, b`` are the
    underlying ``fmpq_poly_t`` corresponding to ``Q, R, A, B``.

Power series division
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr inv_series_newton(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr inv_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr div_series(Fmpq_poly_expr, Fmpq_poly_expr, slong n)

Greatest common divisor
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr gcd(Fmpq_poly_expr, Fmpq_poly_expr)
.. function:: Fmpq_poly_expr lcm(Fmpq_poly_expr, Fmpq_poly_expr)

.. function:: Ltuple<fmpq_polyxx, fmpq_polyxx, fmpq_polyxx>_expr xgcd(Fmpq_poly_expr f, Fmpq_poly_expr g)

    ``ltupleref(G, S, T) = xgcd(A, B)`` has the same effect as
    ``fmpq_poly_xgcd(g, s, t, a, b)``, where ``g, s, t, a, b`` denote the
    underlying ``fmpq_poly_t`` corresponding to ``G, S, T, A, B``.

.. function:: Fmpq_expr resultant(Fmpq_poly_expr)

Derivative and integral
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr derivative(Fmpq_poly_expr)
.. function:: Fmpq_poly_expr integral(Fmpq_poly_expr)

Square roots
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr sqrt_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr invsqrt_series(Fmpq_poly_expr, T:fits_into_slong)

Transcendental functions
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr exp_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr log_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr atan_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr atanh_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr asin_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr asinh_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr tan_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr sin_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr cos_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr sinh_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr cosh_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr tanh_series(Fmpq_poly_expr, T:fits_into_slong)

Evaluation
-------------------------------------------------------------------------------

The overloaded ``operator()`` can be used for evaluation. Additionally we
have the following.

.. function:: Fmpq_expr evaluate(Fmpq_poly_expr, Fmpq_expr)
.. function:: Fmpq_expr evaluate(Fmpq_poly_expr, Fmpz_expr)

Interpolation
-------------------------------------------------------------------------------

.. function:: static Fmpq_poly_expr fmpq_polyxx::interpolate(Fmpz_vec_expr xs, Fmpz_vec_expr ys)

    See ``fmpq_poly_interpolate_fmpq_vec``.

Composition
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr compose(Fmpq_poly_expr, Fmpq_poly_expr)

.. function:: Fmpq_poly_expr rescale(Fmpq_poly_expr, Fmpq_expr)

Power series composition
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr compose_series(Fmpq_poly_expr, Fmpq_poly_expr, slong)
.. function:: Fmpq_poly_expr compose_series_horner(Fmpq_poly_expr, Fmpq_poly_expr, slong)
.. function:: Fmpq_poly_expr compose_series_brent_kung(Fmpq_poly_expr, Fmpq_poly_expr, slong)

Power series reversion
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr revert_series(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr revert_series_newton(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr revert_series_lagrange(Fmpq_poly_expr, T:fits_into_slong)
.. function:: Fmpq_poly_expr revert_series_lagrange_fast(Fmpq_poly_expr, T:fits_into_slong)

Gaussian content
-------------------------------------------------------------------------------

.. function:: Fmpq_expr content(Fmpq_poly_expr)
.. function:: Fmpq_poly_expr primitive_part(Fmpq_poly_expr)
.. function:: bool Fmpq_poly_expr::is_monic() const
.. function:: Fmpq_poly_expr make_monic(Fmpq_poly_expr)

Square-free
-------------------------------------------------------------------------------

.. function:: bool Fmpq_poly_expr::is_squarefree() const

Input and output
-------------------------------------------------------------------------------

.. function:: print(Fmpq_poly_expr)
.. function:: print(FILE*, Fmpq_poly_expr)
.. function:: print_pretty(Fmpq_poly_expr, const char* var)
.. function:: print_pretty(FILE*, Fmpq_poly_expr, const char* var)
.. function:: read(Fmpq_poly_target)
.. function:: read(FILE*, Fmpq_poly_target)


Rational functions (``fmpz_poly_q``)
===============================================================================

Memory management
-------------------------------------------------------------------------------

.. function:: fmpz_poly_qxx::fmpz_poly_qxx()

.. function:: fmpz_poly_qxx::fmpz_poly_qxx(const char*)

    See ``fmpz_poly_q_set_str``.

.. function:: void Fmpz_poly_q_target::canonicalise()
.. function:: bool Fmpz_poly_q_src::is_canonical() const

.. function:: ?? Fmpz_poly_q_expr::num() const
.. function:: ?? Fmpz_poly_q_expr::den() const

    Unified coefficient access to the numerator or denominator of the rational
    function. If this is used for modification, a call to ``canonicalise()``
    may be necessary.

Randomisation
-------------------------------------------------------------------------------

.. function:: static fmpz_poly_qxx fmpz_poly_qxx::randtest(frandxx& state,
        slong len1, mp_bitcnt_t bits1, slong len2, mp_bitcnt_t bits2)
.. function:: static fmpz_poly_qxx fmpz_poly_qxx::randtest_not_zero(frandxx& state,
        slong len1, mp_bitcnt_t bits1, slong len2, mp_bitcnt_t bits2)

    See ``fmpz_poly_q_randtest`` etc.

Assignment
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_q_target Fmpz_poly_q_target::operator=(T:fits_into_slong)

.. function:: void Fmpz_poly_q_target::set_zero()
.. function:: void Fmpz_poly_q_target::set_one()
.. function:: static fmpz_poly_qxx fmpz_poly_qxx::zero()
.. function:: static fmpz_poly_qxx fmpz_poly_qxx::one()

.. function:: Fmpz_poly_q_expr inv(Fmpz_poly_q_expr)
.. function:: Fmpz_poly_q_expr Fmpz_poly_q_expr::inv() const

Comparison
-------------------------------------------------------------------------------

The overloaded operator ``==`` can be used for comparison. Additionally, we
have the following functions.

.. function:: bool Fmpz_poly_q_expr::is_one() const
.. function:: bool Fmpz_poly_q_expr::is_zero() const

Powering
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_q_expr pow(Fmpz_poly_q_expr, T:is_unsigned_integer)
.. function:: Fmpz_poly_q_expr Fmpz_poly_q_expr::pow(T:is_unsigned_integer) const

Derivative
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_q_expr Fmpz_poly_q_expr::derivative() const
.. function:: Fmpz_poly_q_expr derivative(Fmpz_poly_q_expr)

Input and output
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_q_target Fmpz_poly_q_target::operator=(const char*)

    See ``fmpz_poly_q_set_str``.

.. function:: std::string Fmpz_poly_q_expr::to_string() const

    See ``fmpz_poly_q_get_str``.

.. function:: std::string Fmpz_poly_q_expr::pretty(const char* x) const

    See ``fmpz_poly_q_get_str_pretty``.

.. function:: int print(Fmpz_poly_q_expr)
.. function:: int print_pretty(Fmpz_poly_q_expr, const char* var)


Matrices of polynomials over the integers (``fmpz_poly_mat``)
===============================================================================

The class ``fmpz_poly_matxx`` wraps ``fmpz_poly_mat_t``, and so
represents matrices with coefficients in $\mathbf{Z}[X]$. Its usage is similar
to ``fmpz_matxx`` in most regards.

.. function:: Fmpz_poly_mat_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``det``, ``det_fflu``, ``det_interpolate``, ``trace``,
    ``sqr``, ``sqr_classical``, ``sqr_KS``,
    ``transpose``.

.. function:: Fmpz_poly_mat_expr::binary operation(??) const

    The following binary functions are made available as member functions:
    ``solve``, ``solve_fflu``, ``mul_classical``,
    ``mul_interpolate``, ``mul_KS``, ``pow``, ``sqrlow``.

.. function:: Fmpz_poly_mat_expr::three operation(??) const

    The following threeary functions are made available as member functions:
    ``mullow``, ``pow_trunc``.

.. function:: Fmpz_mat_expr Fmpz_poly_mat_expr::operator()(Fmpz_expr) const

    ``operator()`` is overloaded for matrix evaluation.

.. function:: Fmpz_poly_mat_expr operator?(??, ??)

    Arithmetic operators ``+ - *`` are overloaded when provided by
    ``fmpz_poly_mat_t``.

.. function:: Fmpz_poly_mat_expr operator-(Fmpz_poly_mat_expr)

    The unary negation operator is overloaded.

Input and output
-------------------------------------------------------------------------------

.. function:: int print_pretty(Fmpz_poly_mat_expr, const char* x)

Basic properties
-------------------------------------------------------------------------------

.. function:: slong Fmpz_poly_mat_expr::rows() const
.. function:: slong Fmpz_poly_mat_expr::cols() const

    Obtain the number of rows/columns in this matrix. These functions never
    cause evaluation (the matrix size is computed from the operations in the
    expression template and the size of the input matrices).

Basic assignment and manipulation
-------------------------------------------------------------------------------

.. function:: ?? Fmpz_poly_mat_expr::at(T:fits_into_slong, U:fits_into_slong) const

    Unified coefficient access to the matrix entries.

Standard matrices
-------------------------------------------------------------------------------

.. function:: void Fmpz_poly_mat_target::set_zero()
.. function:: void Fmpz_poly_mat_target::set_one()
.. function:: static fmpz_poly_matxx fmpz_poly_matxx::zero(slong rows, slong cols)
.. function:: static fmpz_poly_matxx fmpz_poly_matxx::one(slong rows, slong cols)

Random matrix generation
-------------------------------------------------------------------------------

.. function:: void Fmpz_poly_mat_target::set_randtest(frandxx& state, slong len, mp_bitcnt_t)
.. function:: void Fmpz_poly_mat_target::set_randtest_unsigned(frandxx& state, slong len, mp_bitcnt_t)
.. function:: void Fmpz_poly_mat_target::set_randtest_sparse(frandxx& state, slong len, mp_bitcnt_t, float)
.. function:: static fmpz_poly_matxx fmpz_poly_matxx::randtest(slong rows, slong cols, frandxx&, slong len, mp_bitcnt_t)
.. function:: static fmpz_poly_matxx fmpz_poly_matxx::randtest_unsigned(slong rows, slong cols, frandxx&, slong len, mp_bitcnt_t)
.. function:: static fmpz_poly_matxx fmpz_poly_matxx::randtest_sparse(slong rows, slong cols, frandxx&, slong len, mp_bitcnt_t, float density)

    See ``fmpz_poly_mat_randtest`` etc.

Basic comparison and properties
-------------------------------------------------------------------------------

.. function:: bool Fmpz_poly_mat_expr::is_zero() const
.. function:: bool Fmpz_poly_mat_expr::is_one() const
.. function:: bool Fmpz_poly_mat_expr::is_empty() const
.. function:: bool Fmpz_poly_mat_expr::is_square() const

Norms
-------------------------------------------------------------------------------

.. function:: slong Fmpz_poly_mat_expr::max_length() const
.. function:: slong Fmpz_poly_mat_expr::max_bits() const

Transpose
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_mat_expr transpose(Fmpz_poly_mat_expr)

Arithmetic
-------------------------------------------------------------------------------

Basic arithmetic is most easily done using the overloaded operators
``+ * -``. Finer control can be obtained using the following functions.

.. function:: Fmpz_mat_expr mul_classical(Fmpz_mat_expr, Fmpz_mat_expr)
.. function:: Fmpz_mat_expr mul_KS(Fmpz_mat_expr, Fmpz_mat_expr)
.. function:: Fmpz_poly_mat_expr mullow(Fmpz_poly_mat_expr, Fmpz_poly_mat_expr, slong)
.. function:: Fmpz_poly_mat_expr sqr(Fmpz_poly_mat_expr)
.. function:: Fmpz_poly_mat_expr sqr_KS(Fmpz_poly_mat_expr)
.. function:: Fmpz_poly_mat_expr sqr_classical(Fmpz_poly_mat_expr)
.. function:: Fmpz_poly_mat_expr sqrlow(Fmpz_poly_mat_expr, T:fits_into_slong n)

.. function:: Fmpz_poly_mat_expr pow(Fmpz_poly_mat_expr, T:is_unsigned_integer)
.. function:: Fmpz_poly_mat_expr pow_trunc(Fmpz_poly_mat_expr, T:is_unsigned_integer, T:fits_into_slong)
.. function:: Fmpz_poly_mat_expr prod(Fmpz_poly_mat_vec_expr)

Row reduction
-------------------------------------------------------------------------------

Beware that compared to the C interface, the flintxx row reduction interface
changes some argument orders. This is to facilitate default arguments.

.. function:: slong find_pivot_any(Fmpz_poly_mat_expr, slong, slong, slong)

    See ``fmpz_poly_mat_find_pivot_any``.

.. function:: slong find_pivot_partial(Fmpz_poly_mat_expr, slong, slong, slong)

    See ``fmpz_poly_mat_find_pivot_partial``.

.. function:: Ltuple<slong, fmpz_poly_matxx, fmpzxx>_expr fflu(Fmpz_poly_mat_expr A, permxx* perm = 0, bool rankcheck = false)

    See ``fmpz_poly_mat_fflu``.

.. function:: Ltuple<slong, fmpz_poly_matxx, fmpzxx>_expr rref(Fmpz_poly_mat_expr A)

    See ``fmpz_poly_mat_rref``.

Trace
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr trace(Fmpz_poly_mat_expr)

Determinant and rank
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr det(Fmpz_poly_mat_expr)
.. function:: Fmpz_poly_expr det_fflu(Fmpz_poly_mat_expr)
.. function:: Fmpz_poly_expr det_interpolate(Fmpz_poly_mat_expr)
.. function:: slong rank(Fmpz_poly_mat_expr)

Inverse
-------------------------------------------------------------------------------

.. function:: Ltuple<bool, fmpz_poly_matxx, fmpz_polyxx>_expr inv(Fmpz_poly_mat_expr)

    ``ltupleref(b, M, D) = inv(A)`` has the same effect as
    ``b = fmpz_poly_mat_inv(m, d, a)``, where ``m, d, a`` are the underlying C
    objects corresponding to ``M, D, A``.

Nullspace
-------------------------------------------------------------------------------

.. function:: Ltuple<slong, fmpz_poly_matxx>_expr nullspace(Fmpz_poly_mat_expr A)

    ``ltupleref(n, B) = nullspace(A)`` has the same effect as\\
    ``n = fmpz_poly_mat_nullspace(b, a)``, where ``b, a`` are the underlying
    ``fmpz_poly_mat_t`` corresponding to ``B, A``.

Solving
-------------------------------------------------------------------------------

.. function:: Ltuple<bool, fmpz_poly_matxx, fmpz_polyxx>_expr solve(Fmpz_poly_mat_expr B, Fmpz_poly_mat_expr X)
.. function:: Ltuple<bool, fmpz_poly_matxx, fmpz_polyxx>_expr solve_fflu(Fmpz_poly_mat_expr B, Fmpz_poly_mat_expr X)
.. function:: Ltuple<bool, fmpz_poly_matxx, fmpz_polyxx>_expr solve_fflu_precomp(const permxx&, Fmpz_poly_mat_expr B, Fmpz_poly_mat_expr FFLU, Fmpz_poly_mat_expr X)

    ``ltupleref(w, M, D) = solve(B, X)`` has the same effect as\\
    ``w = fmpz_poly_mat_solve(m, d, b, x)``, where ``m, d, b, x`` are the
    underlying C objects corresponding to ``M, D, B, X``.
    Similarly for the other functions.

Integers mod `n` (``nmod``)
===============================================================================

The class ``nmodxx`` encapsulates the use of ``mp_limb_t`` together with
``nmod_t`` for doing arithmetic modulo a word-sized integer. It is defined
in ``nmod_vecxx.h``.

The C++ equivalent to ``nmod_t`` is ``nmodxx_ctx``. There is a reference
version ``nmodxx_ctx_srcref``.

The C++ equivalent to ``mp_limb_t`` in this context is ``nmodxx``.
Immediate ``nmodxx`` expressions store both an ``mp_limb_t`` and an
``nmodxx_ctx_srcref``.

The most common ways to construct ``nmodxx`` are using the static member
functions ``nmodxx::red`` and ``nmodxx::make_nored``. For convenience,
``operator%`` is overloaded with right hand side ``nmodxx_ctx`` (or
``nmodxx_ctx_srcref``) to call ``nmodxx::red``.

Just like when ``mp_limb_t`` is passed to ``nmod_t`` operations, the
limb stored in ``nmodxx`` is assumed to be reduced, and under this
assumption, all computations yield reduced data.

It is assumed that any expression of ``nmodxx`` involves only one modulus,
so that all contexts are interchangeable.

Miscellaneous
-------------------------------------------------------------------------------

.. function:: explicit nmodxx_ctx::nmodxx_ctx(mp_limb_t n)

    Initialise a new context for operations modulo $n$.

.. function:: nmodxx_ctx_srcref::nmodxx_ctx_srcref(const nmodxx_ctx&)

    Initialise a reference to an ``nmodxx_ctx``.

.. function:: static nmodxx_ctx_srcref::make(const nmod_t& nm)

    Initialise a reference pointing to an ``nmod_t``.

.. function:: const nmod_t& nmodxx_ctx::_nmod() const
.. function:: const nmod_t& nmodxx_ctx_srcref::_nmod() const

    Obtain a reference to the underlying ``nmod_t``.

.. function:: mp_limb_t nmodxx_ctx::n() const
.. function:: mp_limb_t nmodxx_ctx_srcref::n() const

    Obtain the modulus stored in this context.

.. function:: nmodxx::nmodxx(nmodxx_ctx_srcref ctx)

    Initialise an ``nmodxx`` to zero.

.. function:: static nmodxx nmodxx::make_nored(mp_limb_t n, nmodxx_ctx_srcref ctx)

    Initialise an ``nmodxx`` to $n$, performing no reductions.

.. function:: static nmodxx nmodxx::red(mp_limb_t n, nmodxx_ctx_srcref ctx)
.. function:: static nmodxx nmodxx::red(Fmpz_expr n, nmodxx_ctx_srcref ctx)
.. function:: static nmodxx nmodxx::red(Fmpq_expr n, nmodxx_ctx_srcref ctx)

    Initialise an ``nmodxx`` to the reduction of $n$.

.. function:: static nmodxx_ref nmodxx_ref::make(mp_limb_t& l, nmodxx_ctx_srcref c)
.. function:: static nmodxx_srcref nmodxx_srcref::make(const mp_limb_t&, nmodxx_ctx_srcref)

    Obtain a flintxx reference object pointing to ``l``, which is
    interpreted as a limb reduced modulo ``c``.

.. function:: void Nmod_target::reduce()

    Reduce the stored limb.

.. function:: void Nmod_target::set_nored(mp_limb_t n)

    Set the stored limb to $n$.

.. function:: std::string Nmod_expr::to_string() const

    Convert self into a string of the form "a mod b".

.. function:: mp_limb_t Nmod_expr::to<mp_limb_t>() const

    Obtain the stored limb.

.. function:: nmodxx_ctx_srcref Nmod_expr::estimate_ctx() const

    Obtain the context of any immediate subexpression. (By our homogeneity
    assumptions, the result of this operation does not depend on the
    subexpression chosen.)

.. function:: Nmod_expr Nmod_expr::inv() const
.. function:: Nmod_expr Nmod_expr::pow(T:is_unsigned_integer) const

.. function:: Nmod_expr operator??(Nmod_expr, Nmod_expr)

    Arithmetic operators ``+ - * /`` are overloaded for nmod expressions.

.. function:: Nmod_expr operator-(Nmod_expr)
.. function:: Nmod_expr pow(Nmod_expr, T:is_unsigned_integer)
.. function:: Nmod_expr inv(Nmod_expr)

Polynomials over integers mod `n` (``nmod_polyxx``)
===============================================================================

The class ``nmod_polyxx`` wraps ``nmod_poly_t``. Like ``nmodxx``,
instances of ``nmod_polyxx`` always have an associated ``nmodxx_ctx``
storing the operating modulus. No expression may involve more than one
modulus at a time.

In order to reduce convert a ``fmpz_polyxx`` or ``fmpq_polyxx`` to
``nmod_polyxx``, see the ``reduce`` method of ``fmpz_polyxx`` or
``fmpq_polyxx``, respectively.

.. function:: nmodxx_ctx_srcref Nmod_poly_expr::estimate_ctx() const

    Obtain the relevant context. This never causes evaluation.

.. function:: Nmod_poly_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``derivative``, ``integral``, ``make_monic``, ``sqrt``.

.. function:: Nmod_poly_expr::binary operation() const

    The following binary functions are made available as member functions:\\
    ``compose_divconquer``, ``compose_horner``, ``div_basecase``,\\
    ``div_divconquer``, ``div_newton``, ``divrem``,\\
    ``divrem_basecase``, ``divrem_divconquer``,\\
    ``divrem_newton``, ``div_root``, ``evaluate_fast``,\\
    ``evaluate_iter``, ``gcd``, ``gcd_euclidean``, ``gcd_hgcd``,\\
    ``inv_series``, ``inv_series_basecase``, ``inv_series_newton``,\\
    ``invsqrt_series``, ``mul_classical``, ``mul_KS``,\\
    ``shift_left``, ``shift_right``, ``pow``,\\
    ``pow_binexp``, ``rem_basecase``, ``resultant``,\\
    ``resultant_euclidean``, ``reverse``, ``revert_series``,\\
    ``revert_series_lagrange``, ``revert_series_lagrange_fast``,\\
    ``revert_series_newton``, ``sqrt_series``, ``taylor_shift``,\\
    ``taylor_shift_convolution``, ``taylor_shift_horner``, ``xgcd``,\\
    ``xgcd_euclidean``, ``xgcd_hgcd``, ``log_series``,\\
    ``exp_series``, ``exp_series_basecase``, ``atan_series``,\\
    ``atanh_series``, ``asin_series``, ``asinh_series``,\\
    ``sin_series``, ``cos_series``, ``tan_series``,\\
    ``sinh_series``, ``cosh_series``, ``tanh_series``.

.. function:: Nmod_poly_expr Nmod_poly_expr::inflate(T:is_unsigned_integer) const

    ``See inflate``.

.. function:: Nmod_poly_expr Nmod_poly_expr::deflate(T:is_unsigned_integer) const

    ``See deflate``.

.. function:: Nmod_poly_expr::ternary operation(??, ??) const

    The following ternary functions are made available as member functions:\\
    ``compose_mod``, ``compose_mod_horner``,\\
    ``compose_series_brent_kung``, ``compose_series``,\\
    ``compose_series_brent_kung``, ``compose_series_divconquer``,\\
    ``compose_series_horner``, ``div_newton_n_preinv``,\\
    ``divrem_newton_n_preinv``, ``div_series``, ``mulhigh``,\\
    ``mulhigh_classical``, ``mullow``, ``mullow_classical``,\\
    ``mullow_KS``, ``mulmod``, ``powmod_binexp``, ``pow_trunc``,\\
    ``pow_trunc_binexp``.

.. function:: Nmod_poly_expr::fourary operation(??, ??, ??) const

    The following functions of four arguments
    are made available as member functions:
    ``compose_mod_brent_kung_preinv``, ``mulmod_preinv``,
    ``powmod_binexp_preinv``.

.. function:: Nmod_poly_expr Nmod_poly_expr::operator()(Nmod_poly_expr) const
.. function:: Nmod_expr Nmod_poly_expr::operator()(Nmod_expr) const
.. function:: Nmod_vec_expr Nmod_poly_expr::operator()(Nmod_vec_expr) const

    The ``operator()`` is overloaded for evaluation or composition,
    depending on the argument.

.. function:: Nmod_poly_expr operator?(??, ??)

    Arithmetic operators ``+ - * / %`` are overloaded when provided by
    ``nmod_poly_t``.

.. function:: Nmod_poly_expr operator-(Nmod_poly_expr)

    The unary negation operator is overloaded.

.. function:: Nmod_poly_target Nmod_poly_target::operator=(const char*)

    See ``nmod_poly_set_str``. Raises ``flint_exception`` if the string
    is malformed.

Conversion
-------------------------------------------------------------------------------

.. function:: static nmod_polyxx nmod_polyxx::reduce(Fmpz_mod_poly_expr, nmodxx_ctx_srcref)
.. function:: static nmod_polyxx nmod_polyxx::reduce(Fmpq_mod_poly_expr, nmodxx_ctx_srcref)
.. function:: static nmod_polyxx nmod_polyxx::reduce(Fmpz_mod_poly_expr, mp_limb_t)
.. function:: static nmod_polyxx nmod_polyxx::reduce(Fmpq_mod_poly_expr, mp_limb_t)

    See ``fmpz_poly_get_nmod_poly``.

.. function:: static nmod_polyxx nmod_polyxx::from_ground(Nmod_expr e)
.. function:: static nmod_polyxx nmod_polyxx::from_ground(mp_limb_t e, nmodxx_ctx_srcref c)

    Consider $e \in \mathbf{Z}/n\mathbf{Z}$ as an element of
    $\mathbf{Z}/n\mathbf{Z}[X]$.

Input and output
-------------------------------------------------------------------------------

.. function:: print(Nmod_poly_expr)
.. function:: print(FILE*, Nmod_poly_expr)
.. function:: read(Nmod_poly_target)
.. function:: read(FILE*, Nmod_poly_target)

Memory management
-------------------------------------------------------------------------------

.. function:: nmod_polyxx::nmod_polyxx(mp_limb_t modulus)
.. function:: nmod_polyxx::nmod_polyxx(mp_limb_t modulus, slong alloc)
.. function:: nmod_polyxx::nmod_polyxx(nmodxx_ctx_srcref ctx)
.. function:: nmod_polyxx::nmod_polyxx(nmodxx_ctx_srcref ctx, slong alloc)

    Instantiate ``nmod_polyxx`` relative to some modulus. If the second
    argument is provided, space is allocated for ``alloc`` coefficients.

.. function:: nmod_polyxx::nmod_polyxx(const char* str)

    Instantiate ``nmod_polyxx`` from a string representation. The modulus is
    parsed (second integer in the string) and the polynomial is initialised
    with this modulus, then ``nmod_poly_set_str`` is called. Raises
    ``flint_exception`` if the string is malformed.

.. function:: static nmod_polyxx nmod_polyxx::zero(mp_limb_t n)
.. function:: static nmod_polyxx nmod_polyxx::one(mp_limb_t n)

.. function:: void Nmod_poly_target realloc(slong alloc)
.. function:: void Nmod_poly_target::fit_length(slong len)
.. function:: void Nmod_poly_target::_normalise()

Polynomial properties
-------------------------------------------------------------------------------

.. function:: slong Nmod_poly_expr::length() const
.. function:: slong Nmod_poly_expr::degree() const
.. function:: slong Nmod_poly_expr::max_bits() const
.. function:: mp_limb_t Nmod_poly_expr::modulus() const

Assignment and basic manipulation
-------------------------------------------------------------------------------

.. function:: void Nmod_poly_target::truncate(slong)
.. function:: void Nmod_poly_target::set_zero()
.. function:: void Nmod_poly_target::set_one()

.. function:: Nmod_poly_expr reverse(Nmod_poly_expr, T:fits_into_slong)

Randomisation
-------------------------------------------------------------------------------

.. function:: void Nmod_target::set_randtest(frandxx& state, slong len)
.. function:: void Nmod_target::set_randtest_irreducible(frandxx& state, slong len)
.. function:: static nmod_polyxx nmod_polyxx::randtest(mp_limb_t n, frandxx& state, slong len)
.. function:: static nmod_polyxx nmod_polyxx::randtest_irreducible(mp_limb_t n, frandxx& state, slong len)

Getting and setting coefficients
-------------------------------------------------------------------------------

.. function:: Nmodxx_expr Nmod_poly_expr::get_coeff(slong n) const
.. function:: void Nmod_target::set_coeff(slong i, Nmodxx_expr)
.. function:: void Nmod_target::set_coeff(slong i, mp_limb_t)

Input and output
-------------------------------------------------------------------------------

.. function:: std::string Nmod_poly_expr::to_string() const

.. function:: std::ostream& operator<<(std::ostream&, Nmod_poly_expr)

    Output to streams is done by first converting to string.

Comparison
-------------------------------------------------------------------------------

.. function:: bool Nmod_poly_expr::is_one() const
.. function:: bool Nmod_poly_expr::is_zero() const

.. function:: bool operator==(Nmod_poly_expr, Nmod_poly_expr)

Scalar multiplication and division
-------------------------------------------------------------------------------

Scalar multiplication is provided via overloaded ``operator*``.
Additionally, the following functions are implemented:

.. function:: Nmod_poly_expr make_monic(Nmod_poly_expr)

Bit packing and unpacking
-------------------------------------------------------------------------------

.. function:: Fmpz_expr Nmod_poly_expr::bit_pack(T:fits_into_mp_bitcnt_t) const
.. function:: static nmod_polyxx nmod_polyxx::bit_unpack(Fmpz_expr, T:fits_into_mp_bitcnt_t) const

Multiplication
-------------------------------------------------------------------------------

Basic multiplication is provided via overloaded ``operator*``. Finer control
can be obtained using the following functions.

.. function:: Nmod_poly_expr mul_classical(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr mul_KS(Nmod_poly_expr, Nmod_poly_expr)

.. function:: Nmod_poly_expr mullow(Nmod_poly_expr, Nmod_poly_expr, slong)
.. function:: Nmod_poly_expr mullow_classical(Nmod_poly_expr, Nmod_poly_expr, slong)
.. function:: Nmod_poly_expr mullow_KS(Nmod_poly_expr, Nmod_poly_expr, slong)
.. function:: Nmod_poly_expr mulhigh(Nmod_poly_expr, Nmod_poly_expr, slong)
.. function:: Nmod_poly_expr mulhigh_classical(Nmod_poly_expr, Nmod_poly_expr, slong)

.. function:: Nmod_poly_expr mulmod(Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr mulmod_preinv(Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr,
        Nmod_poly_expr)

Powering
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr pow(Nmod_poly_expr, T:is_unsigned_integer)
.. function:: Nmod_poly_expr pow_binexp(Nmod_poly_expr, T:is_unsigned_integer)
.. function:: Nmod_poly_expr pow_trunc(Nmod_poly_expr, T:is_unsigned_integer, T:fits_into_slong)
.. function:: Nmod_poly_expr pow_trunc_binexp(Nmod_poly_expr, T:is_unsigned_integer, T:fits_into_slong)
.. function:: Nmod_poly_expr powmod_binexp(Nmod_poly_expr, T:is_unsigned_integer, Nmod_poly_expr)
.. function:: Nmod_poly_expr powmod_binexp_preinv(Nmod_poly_expr, T:is_unsigned_integer, Nmod_poly_expr, Nmod_poly_expr)

Division
-------------------------------------------------------------------------------

Basic division and remainder is provided by overloaded ``operator/`` and
``operator%``. Finer control can be obtained using the following
functions.

.. function:: Ltuple<nmod_polyxx, nmod_polyxx>_expr divrem(Nmod_poly_expr A, Nmod_poly_expr B)
.. function:: Ltuple<nmod_polyxx, nmod_polyxx>_expr divrem_basecase(Nmod_poly_expr A, Nmod_poly_expr B)
.. function:: Ltuple<nmod_polyxx, nmod_polyxx>_expr divrem_divconquer(Nmod_poly_expr A, Nmod_poly_expr B)
.. function:: Ltuple<nmod_polyxx, nmod_polyxx>_expr divrem_newton(Nmod_poly_expr A, Nmod_poly_expr B)
.. function:: Ltuple<nmod_polyxx, nmod_polyxx>_expr divrem_newton_n_preinv(Nmod_poly_expr A, Nmod_poly_expr B)

.. function:: Nmod_poly_expr div_basecase(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr div_divconquer(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr div_newton(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr div_newton_n_preinv(Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr)

.. function:: Nmod_poly_expr rem_basecase(Nmod_poly_expr, Nmod_poly_expr)

.. function:: Nmod_poly_expr inv_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr inv_series_basecase(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr inv_series_newton(Nmod_poly_expr, T:fits_into_slong)

.. function:: Nmod_poly_expr div_series(Nmod_poly_expr, Nmod_poly_expr, slong n)

.. function:: Nmod_poly_expr div_root(Nmod_poly_expr, Nmod_expr)

Derivative and integral
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr derivative(Nmod_poly_expr)
.. function:: Nmod_poly_expr integral(Nmod_poly_expr)

Evaluation
-------------------------------------------------------------------------------

Basic evaluation and multipoint evaluation can be achieved using the overloaded
``operator()``. Finer control can be obtained using the following functions.

.. function:: Nmod_expr evaluate(Nmod_poly_expr, Nmod_expr)
.. function:: Nmod_vec_expr evaluate(Nmod_poly_expr, Nmod_vec_expr)
.. function:: Nmod_vec_expr evaluate_fast(Nmod_poly_expr, Nmod_vec_expr)
.. function:: Nmod_vec_expr evaluate_iter(Nmod_poly_expr, Nmod_vec_expr)

Interpolation
-------------------------------------------------------------------------------

.. function:: static Nmod_poly_expr fmpz_polyxx::interpolate(Nmod_vec_expr xs, Nmod_vec_expr ys)
.. function:: static Nmod_poly_expr fmpz_polyxx::interpolate_barycentric(Nmod_vec_expr xs, Nmod_vec_expr ys)
.. function:: static Nmod_poly_expr fmpz_polyxx::interpolate_fast(Nmod_vec_expr xs, Nmod_vec_expr ys)
.. function:: static Nmod_poly_expr fmpz_polyxx::interpolate_newton(Nmod_vec_expr xs, Nmod_vec_expr ys)

Composition
-------------------------------------------------------------------------------

Basic composition can be achieved with the overloaded ``operator()``. Finer
control can be obtained using the following functions.

.. function:: Nmod_poly_expr compose(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr compose_horner(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr compose_divconquer(Nmod_poly_expr, Nmod_poly_expr)

Taylor Shift
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr taylor_shift(Nmod_poly_expr, Nmod_expr)
.. function:: Nmod_poly_expr taylor_shift_horner(Nmod_poly_expr, Nmod_expr)
.. function:: Nmod_poly_expr taylor_shift_convolution(Nmod_poly_expr, Nmod_expr)

Modular composition
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr compose_mod(Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr compose_mod_horner(Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr compose_mod_divconquer(Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr compose_mod_brent_kung(Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr compose_mod_brent_kung_preinv(Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr, Nmod_poly_expr)

Greatest common divisor
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr gcd(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr gcd_euclidean(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_poly_expr gcd_hgcd(Nmod_poly_expr, Nmod_poly_expr)

.. function:: Ltuple<nmod_polyxx, nmod_polyxx, nmod_polyxx>_expr xgcd(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Ltuple<nmod_polyxx, nmod_polyxx, nmod_polyxx>_expr xgcd_euclidean(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Ltuple<nmod_polyxx, nmod_polyxx, nmod_polyxx>_expr xgcd_hgcd(Nmod_poly_expr, Nmod_poly_expr)

.. function:: Nmod_expr resultant(Nmod_poly_expr, Nmod_poly_expr)
.. function:: Nmod_expr resultant_euclidean(Nmod_poly_expr, Nmod_poly_expr)

Power series composition
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr compose_series(Nmod_poly_expr, Nmod_poly_expr, slong)
.. function:: Nmod_poly_expr compose_series_horner(Nmod_poly_expr, Nmod_poly_expr, slong)
.. function:: Nmod_poly_expr compose_series_brent_kung(Nmod_poly_expr, Nmod_poly_expr, slong)
.. function:: Nmod_poly_expr compose_series_divconquer(Nmod_poly_expr, Nmod_poly_expr, slong)

Power series reversion
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr revert_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr revert_series_newton(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr revert_series_lagrange(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr revert_series_lagrange_fast(Nmod_poly_expr, T:fits_into_slong)

Square roots
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr sqrt_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr invsqrt_series(Nmod_poly_expr, T:fits_into_slong)

.. function:: Nmod_poly_expr sqrt_series(Nmod_poly_expr p)

    Compute the square root of $p$. Raises ``flint_exception`` if $p$ is not
    a perfect square.

Transcendental functions
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr exp_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr log_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr atan_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr atanh_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr asin_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr asinh_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr tan_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr sin_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr cos_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr sinh_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr cosh_series(Nmod_poly_expr, T:fits_into_slong)
.. function:: Nmod_poly_expr tanh_series(Nmod_poly_expr, T:fits_into_slong)

Products
-------------------------------------------------------------------------------

.. function:: static Nmod_poly_expr fmpz_polyxx::product_roots(Nmod_vec_expr xs)

Inflation and deflation
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr inflate(Nmod_poly_expr, T:is_unsigned_integer)
.. function:: Nmod_poly_expr deflate(Nmod_poly_expr, T:is_unsigned_integer)

.. function:: slong Nmod_poly_expr::deflation() const

Factorisation
-------------------------------------------------------------------------------

.. function:: bool Nmod_poly_expr::is_squarefree() const
.. function:: bool Nmod_poly_expr::is_irreducible() const
.. function:: slong Nmod_poly_target::remove(Nmod_poly_expr)

.. function:: nmod_poly_factorxx::nmod_poly_factorxx()

    Initialise an empty factorisation.

.. function:: nmod_poly_factorxx::nmod_poly_factorxx(const nmod_poly_factorxx& o)

    Copy a factorisation.

.. function:: bool nmod_poly_factorxx::operator==(const nmod_poly_factorxx&)

    Compare two factorisations.

.. function:: ulong nmod_poly_factorxx::size() const

    Return the number of stored factors.

.. function:: slong nmod_poly_factorxx::exp(slong i) const
.. function:: slong& nmod_poly_factorxx::exp(slong i)

    Obtain the exponent of the ith factor.

.. function:: nmod_polyxx_srcref nmod_poly_factorxx::p(slong i) const
.. function:: nmod_polyxx_ref nmod_poly_factorxx::p(slong i)

    Obtain the ith factor.

.. function:: void nmod_poly_factorxx::realloc(slong a)
.. function:: void nmod_poly_factorxx::fit_length(slong a)
.. function:: void nmod_poly_factorxx::insert(Nmod_poly_expr p, slong e)
.. function:: void nmod_poly_factorxx::concat(const nmod_poly_factorxx&)

.. function:: void nmod_poly_factorxx::set_factor(Nmod_poly_expr)
.. function:: void nmod_poly_factorxx::set_factor_cantor_zassenhaus(Nmod_poly_expr)
.. function:: void nmod_poly_factorxx::set_factor_berlekamp(Nmod_poly_expr)
.. function:: void nmod_poly_factorxx::set_factor_kaltofen_shoup(Nmod_poly_expr)
.. function:: void nmod_poly_factorxx::set_factor_with_cantor_zassenhaus(Nmod_poly_expr)
.. function:: void nmod_poly_factorxx::set_factor_with_berlekamp(Nmod_poly_expr)
.. function:: void nmod_poly_factorxx::set_factor_with_kaltofen_shoup(Nmod_poly_expr)
.. function:: void nmod_poly_factorxx::set_factor_squarefree(Nmod_poly_expr)

    Factorise a polynomial and store its factors. See ``nmod_poly_factor`` etc.

.. function:: void nmod_poly_factorxx::set_factor_equal_deg_probab(frandxx&, Nmod_poly_expr, slong)
.. function:: void nmod_poly_factorxx::set_factor_equal_deg(Nmod_poly_expr, slong)

    See ``nmod_poly_factor_equal_deg_prob`` and
    ``nmod_poly_factor_equal_deg``.

.. function:: void nmod_poly_factorxx::set_factor_distinct_deg(Nmod_poly_expr p, std::vector<slong>& degs)

    See ``nmod_poly_factor_distinct_deg``. Note that ``degs`` must have
    sufficient size to hold all factors. The size of ``degs`` is not
    modified.

.. function:: nmod_poly_factorxx factor(Nmod_poly_expr)
.. function:: nmod_poly_factorxx factor_cantor_zassenhaus(Nmod_poly_expr)
.. function:: nmod_poly_factorxx factor_berlekamp(Nmod_poly_expr)
.. function:: nmod_poly_factorxx factor_kaltofen_shoup(Nmod_poly_expr)
.. function:: nmod_poly_factorxx factor_with_cantor_zassenhaus(Nmod_poly_expr)
.. function:: nmod_poly_factorxx factor_with_berlekamp(Nmod_poly_expr)
.. function:: nmod_poly_factorxx factor_with_kaltofen_shoup(Nmod_poly_expr)
.. function:: nmod_poly_factorxx factor_squarefree(Nmod_poly_expr)

Matrices of integers mod `n` (``nmod_mat``)
===============================================================================

The class ``nmod_matxx`` wraps ``nmod_mat_t``. Like ``nmodxx``,
instances of ``nmod_matxx`` always have an associated ``nmodxx_ctx``
storing the operating modulus. No expression may involve more than one modulus
at a time.

Like ``fmpz_matxx``, many operations on ``nmod_matxx`` do not support
aliasing. The details can be found in the documentation of ``nmod_mat_t``.
Since ``nmod_matxx`` does not use temporary merging, evaluation of
subexpressions never creates new aliases.

===============================================================================

.. function:: nmodxx_ctx_srcref Nmod_mat_expr::estimate_ctx() const

    Obtain the relevant context. This never causes evaluation.

.. function:: Nmod_mat_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``inv``, ``transpose``, ``trace``, ``det``.

.. function:: Nmod_mat_expr::binary operation(??) const

    The following binary functions are made available as member functions:
    ``solve``, ``mul_classical``, ``mul_strassen``.

.. function:: Nmod_mat_expr::ternary operation(??, ??) const

    The following ternary functions are made available as member functions:
    ``solve_tril``, ``solve_tril_recursive``,
    ``solve_tril_classical``, ``solve_triu``,
    ``solve_triu_recursive``, ``solve_triu_classical``.

.. function:: Nmod_mat_expr operator?(??, ??)

    Arithmetic operators ``+ - *`` are overloaded when provided by
    ``nmod_mat_t``.

.. function:: Nmod_mat_expr operator-(Nmod_mat_expr)

    The unary negation operator is overloaded.

Conversion
-------------------------------------------------------------------------------

.. function:: static nmod_matxx::reduce(Fmpz_mat_expr, mp_limb_t modulus)

    See ``fmpz_mat_get_nmod_mat``.

Input and output
-------------------------------------------------------------------------------

.. function:: int print(Nmod_mat_expr)

Memory management
-------------------------------------------------------------------------------

.. function:: nmod_matxx::nmod_matxx(slong m, slong n, mp_limb_t modulus)

    See ``nmod_mat_init``.

Basic properties and manipulation
-------------------------------------------------------------------------------

.. function:: ?? Nmod_mat_expr::at(T:fits_into_slong, U:fits_into_slong) const

    Unified coefficient access to the matrix entries.

.. function:: slong Nmod_mat_expr::rows() const
.. function:: slong Nmod_mat_expr::cols() const

    Obtain the number of rows/columns in this matrix. These functions never
    cause evaluation (the matrix size is computed from the operations in the
    expression template and the size of the input matrices).

.. function:: bool Nmod_mat_expr::is_zero() const
.. function:: bool Nmod_mat_expr::is_empty() const
.. function:: bool Nmod_mat_expr::is_square() const
.. function:: mp_limb_t Nmod_mat_expr::modulus() const

.. function:: void Nmod_mat_target::set_zero()
.. function:: static nmod_matxx nmod_matxx::zero(slong rows, slong cols, mp_limb_t modulus)

    See ``nmod_mat_zero``.

Random matrix generation
-------------------------------------------------------------------------------

.. function:: void Nmod_mat_target::set_randtest(frandxx&)
.. function:: void Nmod_mat_target::set_randfull(frandxx&)
.. function:: void Nmod_mat_target::set_randrank(frandxx&, slong rank)
.. function:: void Nmod_mat_target::set_randtril(frandxx&, bool unit)
.. function:: void Nmod_mat_target::set_randtriu(frandxx&, bool unit)

    See ``nmod_mat_randtest`` etc.

.. function:: static nmod_matxx nmod_matxx::randtest(slong rows, slong cols, mp_limb_t M, frandxx&)
.. function:: static nmod_matxx nmod_matxx::randfull(slong rows, slong cols, mp_limb_t M, frandxx&)
.. function:: static nmod_matxx nmod_matxx::randrank(slong rows, slong cols, mp_limb_t M, frandxx&, slong rank)
.. function:: static nmod_matxx nmod_matxx::randtril(slong rows, slong cols, mp_limb_t M, frandxx&, bool unit)
.. function:: static nmod_matxx nmod_matxx::randtriu(slong rows, slong cols, mp_limb_t M, frandxx&, bool unit)

    Static versions of the above.

.. function:: int Nmod_mat_target::set_randpermdiag(frandxx&, const Vec& v)

    ``M.set_randpermdiag(Rand, V)`` has the same effect as
    ``nmod_mat_randpermdiag(m, rand, V._array(), V.size())``, where ``m``
    and ``rand`` are the underlying C structs corresponding to ``M`` and
    ``Rand``.

    One possibility for Vec is ``nmod_vecxx``.

.. function:: void Nmod_target::apply_randops(frandxx&, slong count)

    See ``nmod_mat_randops``.

Transpose
-------------------------------------------------------------------------------

.. function:: Nmod_mat_expr transpose(Nmod_mat_expr)

Matrix multiplication
-------------------------------------------------------------------------------

The overloaded ``operator*`` can be used for both matrix-matrix and
matrix-scalar multiplication. Finer control can be obtained with the following
functions.

.. function:: Nmod_mat_expr mul_classical(Nmod_mat_expr, Nmod_mat_expr)
.. function:: Nmod_mat_expr mul_strassen(Nmod_mat_expr, Nmod_mat_expr)

Trace
-------------------------------------------------------------------------------

.. function:: Nmod_expr trace(Nmod_mat_expr)

Determinant and rank
-------------------------------------------------------------------------------

.. function:: Nmod_expr det(Nmod_mat_expr)
.. function:: slong rank(Nmod_mat_expr)

Inverse
-------------------------------------------------------------------------------

.. function:: Nmod_mat_expr inv(Nmod_mat_expr A)

    Compute the inverse of the square matrix $A$. Raises ``flint_exception``
    if $A$ is singular. The modulus is required to be prime.

Triangular solving
-------------------------------------------------------------------------------

.. function:: Nmod_mat_expr solve_triu(Nmod_mat_expr, Nmod_mat_expr, bool unit)
.. function:: Nmod_mat_expr solve_triu_classical(Nmod_mat_expr, Nmod_mat_expr, bool unit)
.. function:: Nmod_mat_expr solve_triu_recursive(Nmod_mat_expr, Nmod_mat_expr, bool unit)
.. function:: Nmod_mat_expr solve_tril(Nmod_mat_expr, Nmod_mat_expr, bool unit)
.. function:: Nmod_mat_expr solve_tril_classical(Nmod_mat_expr, Nmod_mat_expr, bool unit)
.. function:: Nmod_mat_expr solve_tril_recursive(Nmod_mat_expr, Nmod_mat_expr, bool unit)

Non-singular square solving
-------------------------------------------------------------------------------

.. function:: Nmod_mat_expr solve(Nmod_mat_expr B, Nmod_mat_expr X)
.. function:: Nmod_vec_expr solve(Nmod_mat_expr B, Nmod_vec_expr X)

    See ``nmod_mat_solve`` and ``nmod_mat_solve_vec``.
    Raises ``flint_exception`` if $B$ is singular.

LU decomposition
-------------------------------------------------------------------------------

.. function:: Tuple<slong, permxx> Nmod_mat_target::set_lu(bool rank_check = false)
.. function:: Tuple<slong, permxx> Nmod_mat_target::set_lu_classical(bool rank_check = false)
.. function:: Tuple<slong, permxx> Nmod_mat_target::set_lu_recursive(bool rank_check = false)

    See ``nmod_mat_lu`` etc.

Reduced row echelon form
-------------------------------------------------------------------------------

.. function:: void Nmod_mat_target::set_rref()

Nullspace
-------------------------------------------------------------------------------

.. function:: Ltuple<slong, nmod_matxx>_expr nullspace(Nmod_mat_expr)

Matrices of polynomials over integers mod `n` (``nmod_poly_matxx``)
===============================================================================

The class ``nmod_poly_matxx`` wraps ``nmod_poly_mat_t``. Like
``nmod_matxx``, instances of ``nmod_poly_matxx`` always have an
associated ``nmodxx_ctx`` storing the operating modulus. No expression may
involve more than one modulus at a time.

Contrary to ``nmod_poly_mat_t``, it is \emph{not} valid to use instances
of ``nmod_poly_matxx`` with zero rows or columns.

Like ``fmpz_matxx``, many operations on ``nmod_poly_matxx`` do not
support aliasing. The details can be found in the documentation of
``nmod_poly_mat_t``. Since ``nmod_poly_matxx`` does not use temporary
merging, evaluation of subexpressions never creates new aliases.

Miscellaneous
-------------------------------------------------------------------------------

.. function:: nmodxx_ctx_srcref Nmod_poly_mat_expr::estimate_ctx() const

    Obtain the relevant context. This never causes evaluation.

.. function:: Nmod_poly_mat_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``det``, ``det_fflu``, ``det_interpolate``, ``trace``,
    ``sqr``, ``sqr_classical``, ``sqr_interpolate``, ``sqr_KS``,
    ``transpose``.

.. function:: Nmod_poly_mat_expr::binary operation(??) const

    The following binary functions are made available as member functions:
    ``solve``, ``solve_fflu``, ``mul_classical``,
    ``mul_interpolate``, ``mul_KS``, ``pow``.

.. function:: Nmod_mat_expr Nmod_poly_mat_expr::operator()(Nmod_expr) const

    ``operator()`` is overloaded for matrix evaluation.

.. function:: Nmod_poly_mat_expr operator?(??, ??)

    Arithmetic operators ``+ - *`` are overloaded when provided by
    ``nmod_poly_mat_t``.

.. function:: Nmod_poly_mat_expr operator-(Nmod_poly_mat_expr)

    The unary negation operator is overloaded.

Input and output
-------------------------------------------------------------------------------

.. function:: int print_pretty(Nmod_poly_mat_expr, const char*)

Memory management
-------------------------------------------------------------------------------

.. function:: nmod_poly_matxx::nmod_poly_matxx(slong m, slong n, mp_limb_t modulus)

    See ``nmod_poly_mat_init``.

Basic assignment and manipulation
-------------------------------------------------------------------------------

.. function:: ?? Nmod_poly_mat_expr::at(T:fits_into_slong, U:fits_into_slong) const

    Unified coefficient access to the matrix entries.

Standard matrices
-------------------------------------------------------------------------------

.. function:: static nmod_poly_matxx nmod_poly_matxx::zero(slong rows, slong cols, mp_limb_t n)
.. function:: static nmod_poly_matxx nmod_poly_matxx::one(slong rows, slong cols, mp_limb_t n)
.. function:: void Nmod_poly_mat_target::set_zero()
.. function:: void Nmod_poly_mat_target::set_one()

Random matrix generation
-------------------------------------------------------------------------------

.. function:: void Nmod_poly_mat_target::set_randtest(frandxx&, slong)
.. function:: void Nmod_poly_mat_target::set_randtest_sparse(frandxx&, slong, float)
.. function:: static nmod_poly_matxx nmod_poly_matxx::randtest(slong rows, slong cols, mp_limb_t n, slong len)
.. function:: static nmod_poly_matxx nmod_poly_matxx::randtest_sparse(slong rows, slong cols, mp_limb_t n, slong len, float density)

    See ``nmod_poly_mat_randtest`` etc.

Basic comparison and properties
-------------------------------------------------------------------------------

.. function:: slong Nmod_poly_mat_expr::rows() const
.. function:: slong Nmod_poly_mat_expr::cols() const

    Obtain the number of rows/columns in this matrix. These functions never
    cause evaluation (the matrix size is computed from the operations in the
    expression template and the size of the input matrices).

.. function:: bool Nmod_poly_mat_expr::is_zero() const
.. function:: bool Nmod_poly_mat_expr::is_one() const
.. function:: bool Nmod_poly_mat_expr::is_empty() const
.. function:: bool Nmod_poly_mat_expr::is_square() const
.. function:: mp_limb_t Nmod_poly_mat_expr::modulus() const

Norms
-------------------------------------------------------------------------------

.. function:: slong Nmod_poly_mat_expr::max_length() const

Arithmetic
-------------------------------------------------------------------------------

The overloaded operators ``+ - *`` can be used for both matrix-matrix and
matrix-scalar multiplication, and matrix-matrix addition/subtraction. Finer
control can be obtained with the following functions.

.. function:: Nmod_poly_mat_expr mul_classical(Nmod_poly_mat_expr, Nmod_poly_mat_expr)
.. function:: Nmod_poly_mat_expr mul_interpolate(Nmod_poly_mat_expr, Nmod_poly_mat_expr)

Row reduction
-------------------------------------------------------------------------------

Beware that compared to the C interface, the flintxx row reduction interface
changes some argument orders. This is to facilitate default arguments.

.. function:: slong find_pivot_any(Nmod_poly_mat_expr, slong, slong, slong)

    See ``nmod_poly_mat_find_pivot_any``.

.. function:: slong find_pivot_partial(Nmod_poly_mat_expr, slong, slong, slong)

    See ``nmod_poly_mat_find_pivot_partial``.

.. function:: Ltuple<slong, nmod_poly_matxx, fmpzxx>_expr fflu(Nmod_poly_mat_expr A, permxx* perm = 0, bool rankcheck = false)

    See ``nmod_poly_mat_fflu``.

.. function:: Ltuple<slong, nmod_poly_matxx, fmpzxx>_expr rref(Nmod_poly_mat_expr A)

    See ``nmod_poly_mat_rref``.

Transpose
-------------------------------------------------------------------------------

.. function:: Nmod_poly_mat_expr transpose(Nmod_poly_mat_expr A)

    Compute the transpose of $A$.

Trace
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr trace(Nmod_poly_mat_expr)

Determinant and rank
-------------------------------------------------------------------------------

.. function:: Nmod_poly_expr det(Nmod_poly_mat_expr)
.. function:: Nmod_poly_expr det_fflu(Nmod_poly_mat_expr)
.. function:: Nmod_poly_expr det_interpolate(Nmod_poly_mat_expr)
.. function:: slong rank(Nmod_poly_mat_expr)

Inverse
-------------------------------------------------------------------------------

.. function:: Ltuple<bool, nmod_poly_matxx, nmod_polyxx>_expr inv(Nmod_poly_mat_expr A)

    ``ltupleref(worked, M, P) = inv(A)`` has the same effect as
    ``worked = nmod_poly_mat_inv(m, p, a)``, where ``m, p, a`` are the C
    structs underlying ``M, P, A``.

Nullspace
-------------------------------------------------------------------------------

.. function:: Ltuple<slong, nmod_poly_matxx>_expr nullspace(Nmod_poly_mat_expr)

Solving
-------------------------------------------------------------------------------

.. function:: Ltuple<bool, nmod_poly_matxx, nmod_polyxx>_expr solve(Nmod_poly_mat_expr, Nmod_poly_mat_expr)
.. function:: Ltuple<bool, nmod_poly_matxx, nmod_polyxx>_expr solve_fflu(Nmod_poly_mat_expr, Nmod_poly_mat_expr)
.. function:: Ltuple<bool, nmod_poly_matxx, nmod_polyxx>_expr solve_fflu_precomp(const permxx&, Nmod_poly_mat_expr B, Nmod_poly_mat_expr FFLU, Nmod_poly_mat_expr X)

    ``ltupleref(worked, M, P) = solve(A, X)`` has the same effect as
    ``worked = nmod_poly_mat_solve(m, p, a, x)``, where ``m, p, a, x``
    are the C structs underlying ``M, P, A, X``.

Polynomials over integers mod `n` (``fmpz_mod_poly``)
===============================================================================

.. function:: Fmpz_mod_poly_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``derivative``, ``integral``, ``make_monic``, ``sqr``.

.. function:: Fmpz_mod_poly_expr::binary operation() const

    The following binary functions are made available as member functions:\\
    ``compose_divconquer``, ``compose_horner``, ``div_basecase``,\\
    ``div_divconquer``, ``div_newton``, ``divrem``,\\
    ``divrem_basecase``, ``divrem_divconquer``,\\
    ``divrem``, ``divrem_f``,\\
    ``gcd``, ``gcd_euclidean``, ``gcd_euclidean_f``, ``gcd_f``,\\
    ``gcdinv``, ``invmod``, ``inv_series_newton``,\\
    ``shift_left``, ``shift_right``, ``pow``,\\
    ``rem_basecase``, ``xgcd``, ``xgcd_euclidean``.

.. function:: Fmpz_mod_poly_expr::ternary operation(??, ??) const

    The following ternary functions are made available as member functions:\\
    ``compose_mod``, ``compose_mod_horner``,\\
    ``compose_series_brent_kung``, ``mullow``,\\
    ``mulmod``, ``powmod_binexp``, ``pow_trunc``,\\
    ``pow_trunc_binexp``.

.. function:: Fmpz_mod_poly_expr Fmpz_mod_poly_expr::operator()(Fmpz_mod_poly_expr) const
.. function:: Fmpz_mod_expr Fmpz_mod_poly_expr::operator()(Fmpz_mod_expr) const

    The ``operator()`` is overloaded for evaluation or composition,
    depending on the argument.

.. function:: Fmpz_mod_poly_expr operator?(??, ??)

    Arithmetic operators ``+ - * %`` are overloaded when provided by
    ``nmod_poly_t``.

.. function:: Fmpz_mod_poly_expr operator-(Fmpz_mod_poly_expr)

    The unary negation operator is overloaded.

Input and output
-------------------------------------------------------------------------------

.. function:: print(Fmpz_mod_poly_expr)
.. function:: print(FILE*, Fmpz_mod_poly_expr)
.. function:: print_pretty(Fmpz_mod_poly_expr, const char* var)
.. function:: print_pretty(FILE*, Fmpz_mod_poly_expr, const char* var)
.. function:: read(Fmpz_mod_poly_target)
.. function:: read(FILE*, Fmpz_mod_poly_target)

Memory management
-------------------------------------------------------------------------------

.. function:: fmpz_mod_polyxx::fmpz_mod_polyxx(Fmpz_expr n)
.. function:: fmpz_mod_polyxx::fmpz_mod_polyxx(Fmpz_expr n, slong alloc)

.. function:: void Fmpz_mod_poly_target realloc(slong alloc)
.. function:: void Fmpz_mod_poly_target::fit_length(slong len)
.. function:: void Fmpz_mod_poly_target::_normalise()
.. function:: void Fmpz_mod_poly_target::truncate(slong)

Randomisation
-------------------------------------------------------------------------------

.. function:: void Fmpz_mod_poly_mat_target::set_randtest(frandxx&, slong)
.. function:: void Fmpz_mod_poly_mat_target::set_randtest_irreducible(frandxx&, slong)
.. function:: void Fmpz_mod_poly_mat_target::set_randtest_not_zero(frandxx&, slong)

    See ``fmpz_mod_poly_randtest``, etc.

.. function:: static fmpz_mod_polyxx fmpz_mod_polyxx::randtest(Fmpz_expr, frandx&, slong)
.. function:: static fmpz_mod_polyxx fmpz_mod_polyxx::randtest_not_zero(Fmpz_expr, frandx&, slong)
.. function:: static fmpz_mod_polyxx fmpz_mod_polyxx::randtest_irreducible(Fmpz_expr, frandx&, slong)

    Static versions of the above.

Attributes
-------------------------------------------------------------------------------

.. function:: fmpzxx_srcref Fmpz_mod_poly_mat_expr::modulus() const

    Obtain the relevant modulus. This never causes evaluation.

.. function:: slong Fmpz_mod_poly_expr::length() const
.. function:: slong Fmpz_mod_poly_expr::degree() const

.. function:: ?? Fmpz_mod_poly_expr::lead() const

    Unified coefficient access for the leading coefficient.
    The result is undefined if the length of the polynomial is zero.

Assignment and swap
-------------------------------------------------------------------------------

.. function:: void Fmpz_mod_poly_target::zero_coeffs(slong i, slong j)
.. function:: void Fmpz_mod_poly_target::set_zero()

.. function:: static fmpz_mod_polyxx fmpz_mod_polyxx::zero(Fmpz_expr m)

Conversion
-------------------------------------------------------------------------------

.. function:: Fmpz_mod_poly_target Fmpz_mod_poly_target::operator=(T:is_unsigned_integer)
.. function:: Fmpz_mod_poly_target Fmpz_mod_poly_target::operator=(Fmpz_expr)
.. function:: Fmpz_mod_poly_target Fmpz_mod_poly_target::operator=(Fmpz_poly_expr)

    See ``fmpz_mod_poly_set_ui``, ``fmpz_mod_poly_set_fmpz`` and
    ``fmpz_mod_poly_set_fmpz_poly``.

.. function:: Fmpz_poly_expr Fmpz_mod_poly_expr::to<fmpz_polyxx>() const

Comparison
-------------------------------------------------------------------------------

.. function:: bool Fmpz_mod_poly_expr::is_zero() const

Getting and setting coefficients
-------------------------------------------------------------------------------

.. function:: Fmpz_expr Fmpz_mod_poly_expr::get_coeff(slong n) const
.. function:: void Fmpz_mod_target::set_coeff(slong i, Fmpz_expr)
.. function:: void Fmpz_mod_target::set_coeff(slong i, T:is_unsigned_integer)

Shifting
-------------------------------------------------------------------------------

.. function:: Fmpz_mod_poly_expr shift_left(Fmpz_mod_poly_expr, T:fits_into_slong)
.. function:: Fmpz_mod_poly_expr shift_right(Fmpz_mod_poly_expr, T:fits_into_slong)

Multiplication
-------------------------------------------------------------------------------

The overloaded ``operator*`` can be used for both poly-poly and poly-scalar
multiplication. Finer control can be obtained using the following functions.

.. function:: Fmpz_mod_poly_expr mullow(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr, slong)
.. function:: Fmpz_mod_poly_expr sqr(Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr mulmod(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)

Powering
-------------------------------------------------------------------------------

.. function:: Fmpz_mod_poly_expr pow(Fmpz_mod_poly_expr, T:is_unsigned_integer)
.. function:: Fmpz_mod_poly_expr pow_binexp(Fmpz_mod_poly_expr, T:is_unsigned_integer)
.. function:: Fmpz_mod_poly_expr pow_trunc(Fmpz_mod_poly_expr, T:is_unsigned_integer, T:fits_into_slong)
.. function:: Fmpz_mod_poly_expr pow_trunc_binexp(Fmpz_mod_poly_expr, T:is_unsigned_integer, T:fits_into_slong)
.. function:: Fmpz_mod_poly_expr powmod_binexp(Fmpz_mod_poly_expr, T:is_unsigned_integer, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr powmod_binexp(Fmpz_mod_poly_expr, Fmpz_expr, Fmpz_mod_poly_expr)

Division
-------------------------------------------------------------------------------

The overloaded operators ``/ %`` can be used for division and remainder.
Finer control can be obtained using the following functions.

.. function:: Ltuple<fmpz_mod_polyxx, fmpz_mod_polyxx>_expr divrem(Fmpz_mod_poly_expr A, Fmpz_mod_poly_expr B)
.. function:: Ltuple<fmpz_mod_polyxx, fmpz_mod_polyxx>_expr divrem_basecase(Fmpz_mod_poly_expr A, Fmpz_mod_poly_expr B)
.. function:: Ltuple<fmpz_mod_polyxx, fmpz_mod_polyxx>_expr divrem_divconquer(Fmpz_mod_poly_expr A, Fmpz_mod_poly_expr B)
.. function:: Ltuple<fmpzxx, fmpz_mod_polyxx, fmpz_mod_polyxx>_expr divrem_f(Fmpz_mod_poly_expr A, Fmpz_mod_poly_expr B)

.. function:: Fmpz_mod_poly_expr div_basecase(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr rem_basecase(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr rem(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)

.. function:: slong Fmpz_mod_poly_target::remove(Fmpz_mod_poly_expr)

Power series inversion
-------------------------------------------------------------------------------

.. function:: Fmpz_mod_poly_expr inv_series_newton(Fmpz_mod_poly_expr, T:fits_into_slong)

Greatest common divisor
-------------------------------------------------------------------------------

.. function:: Fmpz_mod_poly_expr gcd(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr gcd_euclidean(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)

.. function:: Ltuple<fmpz_mod_polyxx, fmpz_mod_polyxx, fmpz_mod_polyxx>_expr xgcd(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Ltuple<fmpz_mod_polyxx, fmpz_mod_polyxx, fmpz_mod_polyxx>_expr xgcd_euclidean(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Ltuple<fmpz_mod_polyxx, fmpz_mod_polyxx, fmpz_mod_polyxx>_expr gcdinv(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)

.. function:: Ltuple<fmpzxx, fmpz_mod_polyxx>_expr gcd_f(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Ltuple<fmpzxx, fmpz_mod_polyxx>_expr gcd_euclidean_f(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)

.. function:: Fmpz_mod_poly_expr invmod(Fmpz_mod_poly_expr f, Fmpz_mod_poly_expr g)

    See ``fmpz_mod_poly_invmod``. Raises ``flint_exception`` if $f$ and
    $g$ are not coprime.

Derivative
-------------------------------------------------------------------------------

.. function:: Fmpz_mod_poly_expr derivative(Fmpz_mod_poly_expr)

Evaluation
-------------------------------------------------------------------------------

.. function:: Fmpz_mod_expr evaluate(Fmpz_mod_poly_expr, Fmpz_mod_expr)

Composition
-------------------------------------------------------------------------------

Basic composition can be achieved with the overloaded ``operator()``. Finer
control can be obtained using the following functions.

.. function:: Fmpz_mod_poly_expr compose(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr compose_horner(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr compose_divconquer(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)

Modular composition
-------------------------------------------------------------------------------

.. function:: Fmpz_mod_poly_expr compose_mod(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr compose_mod_horner(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr compose_mod_divconquer(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)
.. function:: Fmpz_mod_poly_expr compose_mod_brent_kung(Fmpz_mod_poly_expr, Fmpz_mod_poly_expr, Fmpz_mod_poly_expr)

Radix conversion
-------------------------------------------------------------------------------

.. function:: fmpz_mod_poly_radixxx::fmpz_mod_poly_radixxx(Fmpz_poly_expr, slong deg)

    Initialise temporary data for radix conversion.
    See ``fmpz_mod_poly_radix_init``.

.. function:: Fmpz_mod_poly_vec_expr Fmpz_mod_poly_expr::radix(const fmpz_mod_poly_radxxx&)
.. function:: Fmpz_mod_poly_vec_expr radix(Fmpz_mod_poly_expr F, const fmpz_mod_poly_radxxx&)

    Perform radix conversion. See ``fmpz_mod_poly_radix``.
    Note that computing the output vector size requires knowing the degree of
    ``F``. In the current implementation, this will result in evaluating
    ``F`` twice. In order to avoid this, pass in ``F`` in evaluated form,
    or do not form expressions requiring temporaries.

Factoring polynomials over the integers mod `n` (``fmpz_mod_poly_factor``)
===============================================================================

.. function:: bool Fmpz_mod_poly_expr::is_squarefree() const
.. function:: bool Fmpz_mod_poly_expr::is_irreducible() const
.. function:: bool Fmpz_mod_poly_expr::is_irreducible_ddf() const
.. function:: bool Fmpz_mod_poly_expr::is_irreducible_rabin() const
.. function:: slong Fmpz_mod_poly_target::remove(Fmpz_mod_poly_expr)

.. function:: fmpz_mod_poly_factorxx::nmod_poly_factorxx()

    Initialise an empty factorisation.

.. function:: fmpz_mod_poly_factorxx::nmod_poly_factorxx(const nmod_poly_factorxx& o)

    Copy a factorisation.

.. function:: bool fmpz_mod_poly_factorxx::operator==(const nmod_poly_factorxx&)

    Compare two factorisations.

.. function:: ulong fmpz_mod_poly_factorxx::size() const

    Return the number of stored factors.

.. function:: slong fmpz_mod_poly_factorxx::exp(slong i) const
.. function:: slong& fmpz_mod_poly_factorxx::exp(slong i)

    Obtain the exponent of the ith factor.

.. function:: fmpz_mod_polyxx_srcref nmod_poly_factorxx::p(slong i) const
.. function:: fmpz_mod_polyxx_ref nmod_poly_factorxx::p(slong i)

    Obtain the ith factor.

.. function:: void fmpz_mod_poly_factorxx::realloc(slong a)
.. function:: void fmpz_mod_poly_factorxx::fit_length(slong a)
.. function:: void fmpz_mod_poly_factorxx::insert(Fmpz_mod_poly_expr p, slong e)
.. function:: void fmpz_mod_poly_factorxx::concat(const nmod_poly_factorxx&)

.. function:: void fmpz_mod_poly_factorxx::set_factor(Fmpz_mod_poly_expr)
.. function:: void fmpz_mod_poly_factorxx::set_factor_cantor_zassenhaus(Fmpz_mod_poly_expr)
.. function:: void fmpz_mod_poly_factorxx::set_factor_berlekamp(Fmpz_mod_poly_expr)
.. function:: void fmpz_mod_poly_factorxx::set_factor_kaltofen_shoup(Fmpz_mod_poly_expr)
.. function:: void fmpz_mod_poly_factorxx::set_factor_squarefree(Fmpz_mod_poly_expr)

    Factorise a polynomial and store its factors. See ``fmpz_mod_poly_factor`` etc.

.. function:: void fmpz_mod_poly_factorxx::set_factor_equal_deg_probab(frandxx&, Fmpz_mod_poly_expr, slong)
.. function:: void fmpz_mod_poly_factorxx::set_factor_equal_deg(Fmpz_mod_poly_expr, slong)

    See ``fmpz_mod_poly_factor_equal_deg_prob`` and
    ``fmpz_mod_poly_factor_equal_deg``.

.. function:: void fmpz_mod_poly_factorxx::set_factor_distinct_deg(Fmpz_mod_poly_expr p,
        std::vector<slong>& degs)

    See ``fmpz_mod_poly_factor_distinct_deg``. Note that ``degs`` must have
    sufficient size to hold all factors. The size of ``degs`` is not
    modified.

.. function:: fmpz_mod_poly_factorxx factor(Fmpz_mod_poly_expr)
.. function:: fmpz_mod_poly_factorxx factor_cantor_zassenhaus(Fmpz_mod_poly_expr)
.. function:: fmpz_mod_poly_factorxx factor_berlekamp(Fmpz_mod_poly_expr)
.. function:: fmpz_mod_poly_factorxx factor_kaltofen_shoup(Fmpz_mod_poly_expr)
.. function:: fmpz_mod_poly_factorxx factor_squarefree(Fmpz_mod_poly_expr)


P-adics (``padic``)
===============================================================================

The type ``padicxx`` wraps the C interface ``padic_t``, and the type
``padicxx_ctx`` wraps ``padic_ctx_t``.

Evaluating composite expressions requires temporary objects, which must be
initialised to a certain precision and with a certain context. The padicxx
library employs the following rules:

- In any compound expression, there must only be one context involved.

- Temporary objects are initialised to the maximum precision of any
  subexpression.

In most use cases, all objects in a compound expression have the same
precision, and so temporary expressions are evaluated to this precision. If
you need temporary subexpressions to be evaluated to higher precision, the
``toN`` method can be used on immediates to increase their effective
precision, thus (potentially) increasing the precision of intermediates.

Context
-------------------------------------------------------------------------------

.. function:: padicxx_ctx::padicxx_ctx(Fmpz_src p, slong min, slong max, padic_print_mode mode)

    Initialize a padic context. See ``padic_ctx_init``.

.. function:: padic_ctx_t& padicxx_ctx::_ctx() const

    Obtain a reference to the underlying C data structure.
    Note that this reference is mutable even if the instance of
    ``padicxx_ctx`` it
    is obtained from is not. This is because the context contains data which is
    not user-visible, and the C functions change them.

    If this is called on a constant instance of ``padicxx_ctx``, you must
    ensure that no user-visible state is changed.

.. function:: padic_print_mode& padicxx_ctx::mode()
.. function:: padic_print_mode padicxx_ctx::mode() const

C++ particulars
-------------------------------------------------------------------------------

.. function:: padicxx_ctx_srcref Padic_src::get_ctx() const
.. function:: padic_ctx_t& Padic_src::_ctx() const

    Obtain a reference to the context of this instance.

.. function:: padicxx_ctx_srcref Padic_expr::estimate_ctx() const

    Obtain a reference to a context occurring in a subexpression. As per the
    first rule in the introduction to this section, all such contexts are the
    same by definition.

.. function:: Padic_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``exp``, ``exp_balanced``, ``exp_rectangular``, ``inv``,
    ``log``, ``log_balanced``, ``log_satoh``, ``sqrt``,
    ``teichmuller``.

.. function:: Padic_expr Padic_expr::pow(T:fits_into_slong) const

.. function:: padicxx_srcref Padic_src::toN(sslong N) const

    Obtain a new version of the operand, with changed effective precision.

Input and output
-------------------------------------------------------------------------------

.. function:: int print(Padic_expr)
.. function:: int print(FILE*, Padic_expr)

Data structures
-------------------------------------------------------------------------------

.. function:: Fmpz_expr Padic_expr::unit() const

    See ``padic_unit``.

.. function:: slong Padic_expr::val() const
.. function:: slong& Padic_target::val()

.. function:: slong Padic_expr::prec() const
.. function:: slong& Padic_target::prec()

    Obtain the precision of this instance. See ``padic_prec``.
    Note that this never requires evaluation.

Memory management
-------------------------------------------------------------------------------

.. function:: padicxx::padicxx(padicxx_ctx_srcref)

    Initialize padic number to default precision. See ``padic_init``.

.. function:: padicxx::padicxx(padicxx_ctx_srcref c, slong N)

    Initialize padic number to precision $N$. See ``padic_init2``.

.. function:: void Padic_target::reduce()

    See ``padic_reduce``.

Randomisation
-------------------------------------------------------------------------------

.. function:: static padicxx padicxx::randtest(frandxx& state, padicxx_ctx_srcref ctx, slong prec = PADIC_DEFAULT_PREC)

.. function:: static padicxx padicxx::randtest_int(frandxx& state, padicxx_ctx_srcref ctx, slong prec = PADIC_DEFAULT_PREC)

.. function:: static padicxx padicxx::randtest_not_zero(frandxx& state, padicxx_ctx_srcref ctx, slong prec = PADIC_DEFAULT_PREC)

    Obtain a random padic number of precision ``prec``. See
    ``padic_randtest``, ``padic_randtest_int`` and
    ``padic_randtest_not_zero``.

Conversion
-------------------------------------------------------------------------------

.. function:: Padic_target Padic_target::operator=(T:is_integer)
.. function:: Padic_target Padic_target::operator=(Fmpz_expr)
.. function:: Padic_target Padic_target::operator=(Fmpq_expr)

.. function:: padicxx padicxx::from_QQ(Fmpq_expr, padicxx_ctx_srcref)
.. function:: padicxx padicxx::from_QQ(Fmpz_expr, padicxx_ctx_srcref)
.. function:: padicxx padicxx::from_QQ(T:is_integer, padicxx_ctx_srcref)
.. function:: padicxx padicxx::from_QQ(Fmpq_expr, padicxx_ctx_srcref, sslong N)
.. function:: padicxx padicxx::from_QQ(Fmpz_expr, padicxx_ctx_srcref, sslong N)
.. function:: padicxx padicxx::from_QQ(T:is_integer, padicxx_ctx_srcref, sslong N)

.. function:: void Padic_target::set_zero()
.. function:: void Padic_target::set_one()

.. function:: padicxx padicxx::zero(padicxx_ctx_srcref)
.. function:: padicxx padicxx::zero(padicxx_ctx_srcref, sslong N)
.. function:: padicxx padicxx::one(padicxx_ctx_srcref)
.. function:: padicxx padicxx::one(padicxx_ctx_srcref, sslong N)

.. function:: bool Padic_expr::is_zero() const
.. function:: bool Padic_expr::is_one() const

.. function:: fmpzxx Padic_expr::to<fmpzxx>() const

    Convert self to ``fmpzxx``, if possible. See ``padic_get_fmpz``.

.. function:: fmpqxx Padic_expr::to<fmpqxx>() const

    Convert self to ``fmpqxx``. See ``padic_get_fmpz``.

.. function:: std::string Fmpz_expr::to_string() const

Arithmetic operations
-------------------------------------------------------------------------------

The overloaded operators ``+ - * / << >>`` can be used for arithmetic
operations, provided these are implemented for ``padic_t``.

.. function:: Padic_expr inv(Padic_expr)

.. function:: Padic_expr sqrt(Padic_expr)

    Compute square root. May raise ``flint_exception`` if no square root
    exists. See ``padic_sqrt``.

.. function:: Padic_expr pow(Padic_expr, T:fits_into_slong)

Exponential
-------------------------------------------------------------------------------

.. function:: Padic_expr exp(Padic_expr)
.. function:: Padic_expr exp_rectangular(Padic_expr)
.. function:: Padic_expr exp_balanced(Padic_expr)

    Compute the exponential function. These may raise ``flint_exception`` if the
    series do not converge.

Logarithm
-------------------------------------------------------------------------------

.. function:: Padic_expr log(Padic_expr)
.. function:: Padic_expr log_rectangular(Padic_expr)
.. function:: Padic_expr log_balanced(Padic_expr)
.. function:: Padic_expr log_satoh(Padic_expr)

    Compute the logarithm function. These may raise ``flint_exception`` if the
    series do not converge.

Polynomials over the `p`-adics (``padic_poly``)
===============================================================================

The type ``padic_polyxx`` wraps ``padic_poly``. Like ``padicxx``,
every instance of ``padic_polyxx`` contains a reference to a context
``padicxx_ctx``, and stores its own precision. The same rules regarding
temporary expressions apply to ``padic_polyxx`` as to ``padicxx``.

C++ particulars
-------------------------------------------------------------------------------

.. function:: padicxx_ctx_srcref Padic_poly_src::get_ctx() const
.. function:: padic_ctx_t& Padic_poly_src::_ctx() const

    Obtain a reference to the context of this instance.

.. function:: padicxx_ctx_srcref Padic_poly_expr::estimate_ctx() const

    Obtain a reference to a context occurring in a subexpression.

.. function:: Padic_poly_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``derivative``.

.. function:: Padic_poly_expr::binary operation() const

    The following binary functions are made available as member functions:
    ``pow``, ``compose_pow``, ``inv_series``, ``shift_left``,
    ``shift_right``.

.. function:: padic_polyxx_srcref Padic_poly_src::toN(sslong N) const

    Obtain a new version of the operand, with changed effective precision.

Input and output
-------------------------------------------------------------------------------

.. function:: int print(Padic_expr)
.. function:: int print(FILE*, Padic_expr)
.. function:: int print_pretty(Padic_expr, const char*)
.. function:: int print_pretty(FILE*, Padic_expr, const char*)

Memory management
-------------------------------------------------------------------------------

.. function:: padic_polyxx::padic_polyxx(padicxx_ctx_srcref)

    Initialise to zero. See ``padic_poly_init``.

.. function:: padic_polyxx::padic_polyxx(padicxx_ctx_srcref, slong prec, slong alloc = 0)

    See ``padic_poly_init2``.

.. function:: void Padic_poly_target realloc(slong alloc)
.. function:: void Padic_poly_target::fit_length(slong len)
.. function:: void Padic_poly_target::canonicalise()
.. function:: void Padic_poly_target::reduce()
.. function:: void Padic_poly_target::truncate(slong)

Polynomial parameters
-------------------------------------------------------------------------------

.. function:: slong Padic_poly_expr::length() const
.. function:: slong Padic_poly_expr::degree() const

.. function:: slong Padic_expr::val() const
.. function:: slong& Padic_target::val()

.. function:: slong Padic_expr::prec() const
.. function:: slong& Padic_target::prec()

Randomisation
-------------------------------------------------------------------------------

.. function:: static padic_polyxx padic_polyxx::randtest(frandxx& state, padicxx_ctx_srcref ctx, slong len, slong prec = PADIC_DEFAULT_PREC)

.. function:: static padic_polyxx padic_polyxx::randtest_val(frandxx& state, padicxx_ctx_srcref ctx, slong len, slong val, slong prec = PADIC_DEFAULT_PREC)

.. function:: static padic_polyxx padic_polyxx::randtest_not_zero(frandxx& state, slong len, padicxx_ctx_srcref ctx, slong prec = PADIC_DEFAULT_PREC)

Assignment and basic manipulation
-------------------------------------------------------------------------------

The overloaded ``operator=`` can be used for assignments. Additionally, we
provide the following functions.

.. function:: padic_polyxx padic_polyxx::from_QQ(T:is_integer, padicxx_ctx_srcref, sslong N)
.. function:: padic_polyxx padic_polyxx::from_QQ(Fmpz_expr, padicxx_ctx_srcref, sslong N)
.. function:: padic_polyxx padic_polyxx::from_QQ(Fmpq_expr, padicxx_ctx_srcref, sslong N)
.. function:: padic_polyxx padic_polyxx::from_QQ(T:is_integer, padicxx_ctx_srcref)
.. function:: padic_polyxx padic_polyxx::from_QQ(Fmpz_expr, padicxx_ctx_srcref)
.. function:: padic_polyxx padic_polyxx::from_QQ(Fmpq_expr, padicxx_ctx_srcref)
.. function:: padic_polyxx padic_polyxx::from_QQX(Fmpz_poly_expr, padicxx_ctx_srcref, sslong N)
.. function:: padic_polyxx padic_polyxx::from_QQX(Fmpq_poly_expr, padicxx_ctx_srcref, sslong N)
.. function:: padic_polyxx padic_polyxx::from_QQX(Fmpz_poly_expr, padicxx_ctx_srcref)
.. function:: padic_polyxx padic_polyxx::from_QQX(Fmpz_poly_expr, padicxx_ctx_srcref)
.. function:: padic_polyxx padic_polyxx::from_ground(Padic_expr)

.. function:: fmpz_polyxx Padic_poly_expr::to<fmpz_polyxx>() const

    Convert to an integer polynomial. Raises ``flint_exception`` if the
    polynomial is not p-adically integral. See ``padic_poly_get_fmpz_poly``.

.. function:: fmpq_polyxx Padic_poly_expr::to<fmpq_polyxx>() const

    See ``padic_poly_get_fmpq_poly``.

.. function:: padic_polyxx padic_polyxx::zero(const padic_polyxx_ctx&)
.. function:: padic_polyxx padic_polyxx::zero(const padic_polyxx_ctx&, sslong N)
.. function:: padic_polyxx padic_polyxx::one(const padic_polyxx_ctx&)
.. function:: padic_polyxx padic_polyxx::one(const padic_polyxx_ctx&, sslong N)

Getting and setting coefficients
-------------------------------------------------------------------------------

.. function:: Padic_expr Padic_poly_expr::get_coeff(slong n) const
.. function:: void Padic_poly_target::set_coeff(slong i, Padic_expr)

Comparison
-------------------------------------------------------------------------------

The overloaded ``operator==`` can be used for comparison.

.. function:: bool Padic_poly_expr::is_zero() const
.. function:: bool Padic_poly_expr::is_one() const

Arithmetic
-------------------------------------------------------------------------------

The overloaded operators ``+ - *`` can be used for arithmetic.

Powering
-------------------------------------------------------------------------------

.. function:: Padic_poly_expr pow(Padic_poly_expr, T:fits_into_slong)

Series inversion
-------------------------------------------------------------------------------

.. function:: Padic_poly_expr inv_series_newton(Padic_poly_expr, T:fits_into_slong)

Derivative
-------------------------------------------------------------------------------

.. function:: Padic_poly_expr derivative(Padic_poly_expr)

Shifting
-------------------------------------------------------------------------------

.. function:: Padic_poly_expr shift_left(Padic_poly_expr, T:fits_into_slong)
.. function:: Padic_poly_expr shift_right(Padic_poly_expr, T:fits_into_slong)

Evaluation and composition
-------------------------------------------------------------------------------

The overloaded ``operator()`` can be used for both evaluation and
composition.

.. function:: Padic_expr evaluate(Padic_poly_expr, Padic_expr)
.. function:: Padic_poly_expr compose(Padic_poly_expr, Padic_poly_expr)
.. function:: Padic_poly_expr compose_pow(Padic_poly_expr, T:fits_into_slong)

Testing
-------------------------------------------------------------------------------

.. function:: bool Padic_poly_src::is_canonical() const
.. function:: bool Padic_poly_src::is_reduced() const

Matrices of $p$-adics (``padic_mat``)
===============================================================================

The type ``padic_matxx`` wraps ``padic_mat``. Like ``padicxx``,
every instance of ``padic_matxx`` contains a reference to a context
``padicxx_ctx``, and stores its own precision. The same rules regarding
temporary expressions apply to ``padic_matxx`` as to ``padicxx``.

C++ particulars
-------------------------------------------------------------------------------

.. function:: padicxx_ctx_srcref Padic_mat_src::get_ctx() const
.. function:: padic_ctx_t& Padic_mat_src::_ctx() const

    Obtain a reference to the context of this instance.

.. function:: padicxx_ctx_srcref Padic_mat_expr::estimate_ctx() const

    Obtain a reference to a context occurring in a subexpression.

.. function:: padic_matxx_srcref Padic_mat_src::toN(sslong N) const

    Obtain a new version of the operand, with changed effective precision.

.. function:: slong Padic_mat_expr::rows() const
.. function:: slong Padic_mat_expr::cols() const

    Obtain the number of rows/columns of this matrix. This never evaluates.

.. function:: slong Padic_mat_expr::val() const
.. function:: slong& Padic_mat_target::val()

.. function:: Padic_mat_expr Padic_mat_expr::transpose() const

Input and output
-------------------------------------------------------------------------------

.. function:: int print(Padic_mat_expr)
.. function:: int print(FILE*, Padic_mat_expr)
.. function:: int print_pretty(Padic_mat_expr)
.. function:: int print_pretty(FILE*, Padic_mat_expr)

Memory management
-------------------------------------------------------------------------------

.. function:: padic_matxx::padic_matxx(padicxx_ctx_srcref, slong rows, slong cols)

    See ``padic_mat_init``.

.. function:: padic_matxx::padic_matxx(padicxx_ctx_srcref, slong rows, slong cols, slong prec)

    See ``padic_mat_init2``.

.. function:: void Padic_mat_target::canonicalise()
.. function:: void Padic_mat_target::reduce()
.. function:: bool Padic_mat_src::is_canonical() const
.. function:: bool Padic_mat_src::is_reduced() const
.. function:: bool Padic_mat_src::is_square() const
.. function:: bool Padic_mat_src::is_empty() const

Basic assignment
-------------------------------------------------------------------------------

Overloaded ``operator=`` can be used for assignment.

.. function:: void Padic_mat_target::set_zero()
.. function:: void Padic_mat_target::set_one()
.. function:: padic_matxx padic_matxx::zero(padicxx_ctx_srcref)
.. function:: padic_matxx padic_matxx::zero(padicxx_ctx_srcref, sslong N)
.. function:: padic_matxx padic_matxx::one(padicxx_ctx_srcref)
.. function:: padic_matxx padic_matxx::one(padicxx_ctx_srcref, sslong N)

Conversion
-------------------------------------------------------------------------------

Converting from a ``fmpq_matxx`` can be done using ``operator=``, or the
following functions.

.. function:: padic_matxx padic_matxx::from_QQ(Fmpq_mat_expr, padicxx_ctx_srcref)
.. function:: fmpq_matxx Padic_mat_expr::to<fmpq_matxx>() const

Entries
-------------------------------------------------------------------------------

.. function:: ?? Padic_mat_expr::at(slong i, slong j)

    Unified coefficient access to the underlying integer matrix. See
    ``padic_mat_entry``.

.. function:: Fmpz_expr Padic_mat_expr::get_entry(slong i, slong j)
.. function:: void Padic_mat_target::set_entry(slong i, slong j, Padic_expr)

Comparison
-------------------------------------------------------------------------------

Overloaded ``operator==`` can be used for comparison.

.. function:: bool Padic_mat_expr::is_zero() const

Random matrix generation
-------------------------------------------------------------------------------

.. function:: static padic_polyxx padic_polyxx::randtest(slong rows, slong cols,
        frandxx& state, padicxx_ctx_srcref ctx, slong prec = PADIC_DEFAULT_PREC)

Transpose
-------------------------------------------------------------------------------

.. function:: Padic_mat_expr transpose(Padic_mat_expr)

Arithmetic
-------------------------------------------------------------------------------

Overloaded operators ``+ - * /`` can be used for arithmetic.

Q-adics (``qadic``)
===============================================================================

The type ``qadicxx`` wraps the C interface ``qadic_t``, and the type
``qadicxx_ctx`` wraps ``qadic_ctx_t``.

Evaluating composite expressions requires temporary objects, which must be
initialised to a certain precision and with a certain context. The same rules
apply as for ``padicxx``.

Context
-------------------------------------------------------------------------------

.. function:: qadicxx_ctx::qadicxx_ctx(Fmpz_src p, sslong min, slong max, padic_print_mode mode, const char* var = "x")

    Initialize a qadic context. See ``qadic_ctx_init_conway``.

.. function:: qadic_ctx_t& qadicxx_ctx::_ctx() const

    Obtain a reference to the underlying C data structure.
    Note that this reference is mutable even if the instance of
    ``qadicxx_ctx`` it
    is obtained from is not. This is because the context contains data which is
    not user-visible, and the C functions change them.

    If this is called on a constant instance of ``qadicxx_ctx``, you must
    ensure that no user-visible state is changed.

.. function:: padicxx_ctx_srcref qadicxx_ctx::pctx() const

    Obtain a reference to the underlying padic context.

C++ particulars
-------------------------------------------------------------------------------

.. function:: padicxx_ctx_srcref Qadic_src::get_ctx() const
.. function:: const qadicxx_ctx& Qadic_src::get_qctx() const
.. function:: qadic_ctx_t& Qadic_src::_ctx() const

    Obtain a reference to the context of this instance.

.. function:: const qadicxx_ctx& Qadic_expr::estimate_ctx() const

    Obtain a reference to a context occurring in a subexpression. As per the
    first rule in the introduction to this section, all such contexts are the
    same by definition.

.. function:: Qadic_expr::unary operation() const

    The following unary functions are made available as member functions:
    ``exp``, ``exp_balanced``, ``exp_rectangular``, ``inv``,
    ``log``, ``log_balanced``, ``teichmuller``, ``trace``,
    ``norm``, ``norm_analytic``, ``norm_resultant``.

.. function:: Qadic_expr Qadic_expr::pow(Fmpz_expr) const
.. function:: Qadic_expr Qadic_expr::frobenius(T:fits_into_slong) const

.. function:: qadicxx_srcref Qadic_src::toN(sslong N) const

    Obtain a new version of the operand, with changed effective precision.

Data structures
-------------------------------------------------------------------------------

.. function:: int print_pretty(Qadic_expr)
.. function:: int print_pretty(FILE*, Qadic_expr)

Data structures
-------------------------------------------------------------------------------

.. function:: slong Qadic_expr::val() const

.. function:: slong Qadic_expr::prec() const

    Obtain the precision of this instance. See ``qadic_prec``.
    Note that this never requires evaluation.

Memory management
-------------------------------------------------------------------------------

.. function:: qadicxx::qadicxx(const qadicxx_ctx&)

    Initialize qadic number to default precision. See ``qadic_init``.

.. function:: qadicxx::qadicxx(const qadicxx_ctx& c, slong N)

    Initialize qadic number to precision $N$. See ``qadic_init2``.

.. function:: void Qadic_target::reduce()

    See ``qadic_reduce``.

Randomisation
-------------------------------------------------------------------------------

.. function:: static qadicxx qadicxx::randtest(frandxx& state, const qadicxx_ctx& ctx, slong prec = PADIC_DEFAULT_PREC)

.. function:: static qadicxx qadicxx::randtest_int(frandxx& state, slong val, const qadicxx_ctx& ctx, slong prec = PADIC_DEFAULT_PREC)

.. function:: static qadicxx qadicxx::randtest_val(frandxx& state, const qadicxx_ctx& ctx, slong prec = PADIC_DEFAULT_PREC)

.. function:: static qadicxx qadicxx::randtest_not_zero(frandxx& state, const qadicxx_ctx& ctx, slong prec = PADIC_DEFAULT_PREC)

    Obtain a random qadic number of precision ``prec``. See
    ``qadic_randtest``, ``qadic_randtest_int`` and
    ``qadic_randtest_not_zero``.

Conversion
-------------------------------------------------------------------------------

.. function:: Qadic_target Qadic_target::operator=(T:is_unsigned_integer)
.. function:: Qadic_target Qadic_target::operator=(Padic_expr)

.. function:: qadicxx qadicxx::from_ground(Padic_expr, const qadicxx_ctx&)

.. function:: void Qadic_target::set_zero()
.. function:: void Qadic_target::set_one()
.. function:: void Qadic_target::set_gen(const qadicxx_ctx&)

.. function:: qadicxx qadicxx::zero(const qadicxx_ctx&)
.. function:: qadicxx qadicxx::zero(const qadicxx_ctx&, sslong N)
.. function:: qadicxx qadicxx::one(const qadicxx_ctx&)
.. function:: qadicxx qadicxx::one(const qadicxx_ctx&, sslong N)
.. function:: qadicxx qadicxx::gen(const qadicxx_ctx&)
.. function:: qadicxx qadicxx::gen(const qadicxx_ctx&, sslong N)

.. function:: bool Qadic_expr::is_zero() const
.. function:: bool Qadic_expr::is_one() const

.. function:: padicxx Qadic_expr::to<padicxx>() const

    Convert self to ``padicxx``, if possible. See ``qadic_get_padic``.

Arithmetic operations
-------------------------------------------------------------------------------

The overloaded operators ``+ - * / << >>`` can be used for arithmetic
operations, provided these are implemented for ``qadic_t``.

.. function:: Qadic_expr inv(Qadic_expr)

.. function:: Qadic_expr pow(Qadic_expr, Fmpz_expr)

Exponential
-------------------------------------------------------------------------------

.. function:: Qadic_expr exp(Qadic_expr)
.. function:: Qadic_expr exp_rectangular(Qadic_expr)
.. function:: Qadic_expr exp_balanced(Qadic_expr)

    Compute the exponential function. These may raise ``flint_exception``
    if the series do not converge.

Logarithm
-------------------------------------------------------------------------------

.. function:: Qadic_expr log(Qadic_expr)
.. function:: Qadic_expr log_balanced(Qadic_expr)

    Compute the logarithm function. These may raise ``flint_exception``
    if the series do not converge.

Special functions
-------------------------------------------------------------------------------

.. function:: Qadic_expr teichmuller(Qadic_expr)
.. function:: Padic_expr trace(Qadic_expr)
.. function:: Padic_expr norm(Qadic_expr)
.. function:: Padic_expr norm_analytic(Qadic_expr)
.. function:: Padic_expr norm_resultant(Qadic_expr)

Arithmetic functions (``arith``)
===============================================================================

The ``arithxx`` module wraps the ``arith`` module, i.e. provides
functions for computing number theoretic functions.

Primorials
-------------------------------------------------------------------------------

.. function:: Fmpz_expr primorial(T:fits_into_slong)

Harmonic numbers
-------------------------------------------------------------------------------

.. function:: Fmpq_expr harmonic_number(T:fits_into_slong)

Stirling numbers
-------------------------------------------------------------------------------

.. function:: Fmpz_expr stirling_number_1u(T:fits_into_slong, T:fits_into_slong)
.. function:: Fmpz_expr stirling_number_1(T:fits_into_slong, T:fits_into_slong)
.. function:: Fmpz_expr stirling_number_2(T:fits_into_slong, T:fits_into_slong)

.. function:: Fmpz_vec_expr stirling_number_1u_vec(T:fits_into_slong, T:fits_into_slong)
.. function:: Fmpz_vec_expr stirling_number_1_vec(T:fits_into_slong, T:fits_into_slong)
.. function:: Fmpz_vec_expr stirling_number_2_vec(T:fits_into_slong, T:fits_into_slong)

.. function:: Fmpz_vec_expr stirling_number_1u_vec_next(Fmpz_vec_expr v, T:fits_into_slong n)
.. function:: Fmpz_vec_expr stirling_number_1_vec_next(Fmpz_vec_expr v, T:fits_into_slong n)
.. function:: Fmpz_vec_expr stirling_number_2_vec_next(Fmpz_vec_expr v, T:fits_into_slong n)

    Given the vector $v$ of length $k$, compute the next vector of Stirling
    numbers. The size of the new vector is $k + 1$ if $k = n$, and else $k$.

    See ``arith_stirling_number_1u_vec_next`` etc.

.. function:: Fmpz_mat_expr stirling_matrix_1u(T:fits_into_slong m, T:fits_into_slong)
.. function:: Fmpz_mat_expr stirling_matrix_1(T:fits_into_slong m, T:fits_into_slong)
.. function:: Fmpz_mat_expr stirling_matrix_2(T:fits_into_slong m, T:fits_into_slong)

    Compute an $m \times n$ Stirling matrix.

    See ``arith_stirling_matrix_1u`` etc.

Bell numbers
-------------------------------------------------------------------------------

.. function:: Fmpz_expr bell_number(T:is_unsigned_integer)
.. function:: Fmpz_expr bell_number_bsplit(T:is_unsigned_integer)
.. function:: Fmpz_expr bell_number_multi_mod(T:is_unsigned_integer)
.. function:: Fmpz_vec_expr bell_number_vec(T:is_unsigned_integer)
.. function:: Fmpz_vec_expr bell_number_vec_recursive(T:is_unsigned_integer)
.. function:: Fmpz_vec_expr bell_number_vec_multi_mod(T:is_unsigned_integer)

.. function:: Nmod_expr bell_number_nmod(T:is_unsigned_integer, Nmodxx_ctx_src)
.. function:: Nmod_vec_expr bell_number_nmod_vec(T:is_unsigned_integer, Nmodxx_ctx_src)
.. function:: Nmod_vec_expr bell_number_nmod_vec_recursive(T:is_unsigned_integer, Nmodxx_ctx_src)
.. function:: Nmod_vec_expr bell_number_nmod_vec_series(T:is_unsigned_integer, Nmodxx_ctx_src)

.. function:: double bell_number_size(ulong n)

Bernoulli numbers and polynomials
-------------------------------------------------------------------------------

.. function:: Fmpq_expr bernoulli_number(T:is_unsigned_integer)
.. function:: Fmpq_vec_expr bernoulli_number_vec(T:fits_into_slong)
.. function:: Fmpz_expr bernoulli_number_denom(T:is_unsigned_integer)
.. function:: double bernoulli_number_size(ulong)
.. function:: Fmpq_poly_expr bernoulli_polynomial(T:is_unsigned_integer)

Euler numbers and polynomials
-------------------------------------------------------------------------------

.. function:: Fmpq_expr euler_number(T:is_unsigned_integer)
.. function:: Fmpq_vec_expr euler_number_vec(T:fits_into_slong)
.. function:: double euler_number_size(ulong)
.. function:: Fmpq_poly_expr euler_polynomial(T:is_unsigned_integer)

Legendre polynomials
-------------------------------------------------------------------------------

.. function:: Fmpq_poly_expr legendre_polynomial(T:is_unsigned_integer)
.. function:: Fmpz_poly_expr chebyshev_t_polynomial(T:is_unsigned_integer)
.. function:: Fmpz_poly_expr chebyshev_u_polynomial(T:is_unsigned_integer)

Multiplicative functions
-------------------------------------------------------------------------------

.. function:: Fmpz_expr euler_phi(Fmpz_expr)
.. function:: int moebius_mu(Fmpz_expr)
.. function:: Fmpz_expr divisor_sigma(Fmpz_expr, ulong)
.. function:: Fmpz_poly_expr divisors(Fmpz_expr)
.. function:: Fmpz_expr ramanujan_tau(Fmpz_expr)
.. function:: Fmpz_poly_expr ramanujan_tau_series(T:fits_into_slong)

Cyclotomic polynomials
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr cyclotomic_polynomial(T:is_unsigned_integer)
.. function:: Fmpz_poly_expr cos_minpoly(T:is_unsigned_integer)

Swinnerton-Dyer polynomials
-------------------------------------------------------------------------------

.. function:: Fmpz_poly_expr swinnerton_dyer_polynomial(T:is_unsigned_integer)

Landau's function
-------------------------------------------------------------------------------

.. function:: Fmpz_vec_expr landau_function_vec(T:is_unsigned_integer)

Dedekind sums
-------------------------------------------------------------------------------

.. function:: Fmpz_expr dedekind_sum_naive(Fmpz_expr, Fmpz_expr)
.. function:: Fmpz_expr dedekind_sum(Fmpz_expr, Fmpz_expr)

Number of partitions
-------------------------------------------------------------------------------

.. function:: Fmpz_vec_expr number_of_partitions_vec(T:fits_into_slong)
.. function:: Nmod_vec_expr number_of_partitions_nmod_vec(T:fits_into_slong)
.. function:: Fmpz_expr number_of_partitions(T:is_unsigned_integer)

Sums of squares
-------------------------------------------------------------------------------

.. function:: Fmpz_expr sum_of_squares(T:is_unsigned_integer, Fmpz_expr)
.. function:: Fmpz_vec_expr sum_of_squares(T:is_unsigned_integer, T:fits_into_slong)

