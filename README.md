LaTeX 수식 문법에 대한 공식 문서를 참조하여 전체 EBNF 정의를 개선했습니다. 주요 참고 문서로는 AMS-LaTeX 가이드, CTAN의 amsmath 패키지 문서, 그리고 Short Math Guide for LaTeX가 사용되었습니다.

### LaTeX 수식 문법 EBNF 개선된 버전

```ebnf
latex_math_expressions = 
    { arithmetic_operators }
    | { relational_operators }
    | { set_operators }
    | { logical_operators }
    | { miscellaneous_operators }
    | { greek_letters }
    | { symbols }
    | { structures };

arithmetic_operators = 
    "+" | "-" | "*" | "/" | "\cdot" | "\times" | "\div";

relational_operators = 
    "=" | "\neq" | "\ne" | "<" | ">" | "\leq" | "\le" | "\geq" | "\ge"
    | "\approx" | "\equiv" | "\sim" | "\simeq" | "\cong" | "\propto"
    | "\prec" | "\succ" | "\preceq" | "\succeq";

set_operators = 
    "\cup" | "\cap" | "\setminus" | "\in" | "\notin" | "\subset" | "\supset"
    | "\subseteq" | "\supseteq" | "\emptyset" | "\uplus" | "\biguplus";

logical_operators = 
    "\land" | "\lor" | "\neg" | "\implies" | "\iff" | "\forall" | "\exists";

miscellaneous_operators = 
    "\partial" | "\nabla" | "\infty" | "\angle" | "\measuredangle" | "\sphericalangle"
    | "\therefore" | "\because" | "\vdots" | "\cdots" | "\ddots" | "\aleph";

greek_letters = 
    greek_lowercase | greek_uppercase;

greek_lowercase = 
    "\alpha" | "\beta" | "\gamma" | "\delta" | "\epsilon" | "\zeta" | "\eta"
    | "\theta" | "\iota" | "\kappa" | "\lambda" | "\mu" | "\nu" | "\xi"
    | "\omicron" | "\pi" | "\rho" | "\sigma" | "\tau" | "\upsilon"
    | "\phi" | "\chi" | "\psi" | "\omega";

greek_uppercase = 
    "\Alpha" | "\Beta" | "\Gamma" | "\Delta" | "\Epsilon" | "\Zeta"
    | "\Eta" | "\Theta" | "\Iota" | "\Kappa" | "\Lambda" | "\Mu" | "\Nu"
    | "\Xi" | "\Omicron" | "\Pi" | "\Rho" | "\Sigma" | "\Tau" | "\Upsilon"
    | "\Phi" | "\Chi" | "\Psi" | "\Omega";

symbols = 
    "\sum" | "\int" | "\oint" | "\prod" | "\coprod" | "\partial" | "\nabla" | "\infty";

structures = 
    fraction
    | square_root
    | nth_root
    | binomial_coefficient
    | exponent
    | subscript
    | matrix
    | equation_environment
    | bracket
    | overline
    | underline
    | text
    | accent;

fraction = 
    "\frac{" expression "}{" expression "}"
    | "\dfrac{" expression "}{" expression "}"
    | "\tfrac{" expression "}{" expression "}";

square_root = 
    "\sqrt{" expression "}";

nth_root = 
    "\sqrt[" expression "]{" expression "}";

binomial_coefficient = 
    "\binom{" expression "}{" expression "}";

exponent = 
    expression "^" "{" expression "}"
    | expression "^" expression;

subscript = 
    expression "_" "{" expression "}"
    | expression "_" expression;

matrix = 
    "\begin{" matrix_type "} " matrix_elements "\end{" matrix_type "}";

matrix_type = 
    "matrix" | "pmatrix" | "bmatrix" | "Bmatrix" | "vmatrix" | "Vmatrix";

matrix_elements = 
    expression { "&" expression } { "\\" expression { "&" expression } };

equation_environment = 
    "\begin{" equation_type "} " expression "\end{" equation_type "}";

equation_type = 
    "equation" | "equation*" | "align" | "align*" | "multline" | "multline*";

bracket = 
    "\left(" expression "\right)"
    | "\left[" expression "\right]"
    | "\left\{" expression "\right\}"
    | "\left|" expression "\right|"
    | "\left\langle" expression "\right\rangle"
    | "\left\lfloor" expression "\right\rfloor"
    | "\left\lceil" expression "\right\rceil";

overline = 
    "\overline{" expression "}";

underline = 
    "\underline{" expression "}";

text = 
    "\text{" text_content "}";

accent = 
    "\hat{" expression "}"
    | "\bar{" expression "}"
    | "\vec{" expression "}"
    | "\dot{" expression "}"
    | "\ddot{" expression "}"
    | "\tilde{" expression "}"
    | "\breve{" expression "}"
    | "\check{" expression "}"
    | "\acute{" expression "}"
    | "\grave{" expression "}";

expression = 
    { arithmetic_operators }
    | { relational_operators }
    | { set_operators }
    | { logical_operators }
    | { miscellaneous_operators }
    | { greek_letters }
    | { symbols }
    | { structures }
    | { text };
```

이 문법 정의는 AMS-LaTeX 가이드 및 CTAN의 amsmath 패키지 문서에서 제공하는 정보를 바탕으로 하여, LaTeX 수식의 다양한 구성 요소를 포괄적으로 다루도록 개선되었습니다.

참고 문서:
- [User's Guide for the amsmath Package](https://www.ams.org/publications/authors/tex/amslatex)
- [Short Math Guide for LaTeX](https://tug.ctan.org/info/short-math-guide/short-math-guide.pdf)
- [AMS-LaTeX on CTAN](https://ctan.org/pkg/amsmath)
