@mainpage Welcome to SpaceHub

 The website of SpaceHub is still under construction. If you have any question/bug, please issue a report [here](https://github.com/YihanWangAstro/SpaceHub/issues/new?assignees=&labels=&template=bug_report.md&title=).

 [Feature requests](https://github.com/YihanWangAstro/SpaceHub/issues/new?assignees=&labels=&template=feature_request.md&title=) are welcome. 
 
 @section intro Introduction

[Git repository](https://github.com/YihanWangAstro/SpaceHub)
 Introduction

 @section methods Available Integration methods in SpaceHub

| Method name                          | Order    | Stepsize control  | floating point number type | Description                                                |
| ------------------------------------ | -------- | ----------------- | -------------------------- | ---------------------------------------------------------- |
| Sym2                                 | 2        | adaptive          | double                     | 2nd order symplectic method                                |
| Sym2_Ext                             | 2        | adaptive          | long double                | with extended double precision number                      |
| Sym2_Plus                            | 2        | adaptive          | double_k                   | with active round off error compensation                   |
| Sym2_ExtPlus                         | 2        | adaptive          | long_double_k              | extended precision with compensation                       |
| Const_Sym2                           | 2        | constant          | double                     | constant step size                                         |
| Const_Sym2_Ext                       | 2        | constant          | long double                | constant step size                                         |
| Const_Sym2_Plus                      | 2        | constant          | double_k                   | constant step size                                         |
| Const_Sym2_ExtPlus                   | 2        | constant          | long_double_k              | constant step size                                         |
| <Const\_>Sym4<\_Ext><Plus>           | 4        | adaptive/constant | double/long double/...     | 4th order symplectic method with*                          |
| <Const\_>Sym6<\_Ext><Plus>           | 6        | adaptive/constant | double/long double/...     | 6th order symplectic method with*                          |
| <Const\_>Sym8<\_Ext><Plus>           | 8        | adaptive/constant | double/long double/...     | 8th order symplectic method with*                          |
| <Const\_>Sym10<\_Ext><Plus>          | 10       | adaptive/constant | double/long double/...     | 10th order symplectic method with*                         |
| <Const\_>AR_Sym2<\_Ext><Plus>        | 4        | adaptive/constant | double/long double/...     | Algorithmic regularized 2nd order symplectic method with*  |
| <Const\_>AR_Sym4<\_Ext><Plus>        | 4        | adaptive/constant | double/long double/...     | Algorithmic regularized 4th order symplectic method with*  |
| <Const\_>AR_Sym6<\_Ext><Plus>        | 6        | adaptive/constant | double/long double/...     | Algorithmic regularized 6th order symplectic method with*  |
| <Const\_>AR_Sym8<\_Ext><Plus>        | 8        | adaptive/constant | double/long double/...     | Algorithmic regularized 8th order symplectic method with*  |
| <Const\_>AR_Sym10<\_Ext><Plus>       | 10       | adaptive/constant | double/long double/...     | Algorithmic regularized 10th order symplectic method with* |
| <Const\_>Chain_Sym2<\_Ext><Plus>     | 4        | adaptive/constant | double/long double/...     | Chain coordinated 2nd order symplectic method with*        |
| <Const\_>Chain_Sym4<\_Ext><Plus>     | 4        | adaptive/constant | double/long double/...     | Chain coordinated 4th order symplectic method with*        |
| <Const\_>Chain_Sym6<\_Ext><Plus>     | 6        | adaptive/constant | double/long double/...     | Chain coordinated 6th order symplectic method with*        |
| <Const\_>Chain_Sym8<\_Ext><Plus>     | 8        | adaptive/constant | double/long double/...     | Chain coordinated 8th order symplectic method with*        |
| <Const\_>Chain_Sym10<\_Ext><Plus>    | 10       | adaptive/constant | double/long double/...     | Chain coordinated 10th order symplectic method with*       |
| <Const\_>AR_Sym2_Chain<\_Ext><Plus>  | 4        | adaptive/constant | double/long double/...     | AR 2nd order symplectic Chain method with*                 |
| <Const\_>AR_Sym4_Chain<\_Ext><Plus>  | 4        | adaptive/constant | double/long double/...     | AR 4th order symplectic Chain method with*                 |
| <Const\_>AR_Sym6_Chain<\_Ext><Plus>  | 6        | adaptive/constant | double/long double/...     | AR 6th order symplectic Chain method with*                 |
| <Const\_>AR_Sym8_Chain<\_Ext><Plus>  | 8        | adaptive/constant | double/long double/...     | AR 8th order symplectic Chain method with*                 |
| <Const\_>AR_Sym10_Chain<\_Ext><Plus> | 10       | adaptive/constant | double/long double/...     | AR 10th order symplectic Chain method with*                |
| BS<\_Ext><Plus>                      | adaptive | adaptive          | double/long double/...     | Bulirsch-Stoer extrapolation method with*                  |
| AR_BS<\_Ext><Plus>                   | adaptive | adaptive          | double/long double/...     | Algorithmic regularized Bulirsch-Stoer method with*        |
| Chain_BS<\_Ext><Plus>                | adaptive | adaptive          | double/long double/...     | Chain coordinated Bulirsch-Stoer method with*              |
| AR_Chain<\_Ext><Plus>                | adaptive | adaptive          | double/long double/...     | AR-Chain method with*                                      |
| <Const\_>Radau<\_Ext><Plus>          | 15       | adaptive/constant | double/long double/...     | Gauss-Radau method with*                                   |
| <Const\_>AR_Radau<\_Ext><Plus>       | 15       | adaptive/constant | double/long double/...     | Algorithmic regularized Gauss-Radau method with*           |
| <Const\_>Chain_Radau<\_Ext><Plus>    | 15       | adaptive/constant | double/long double/...     | Chain coordinated Gauss-Radau method with*                 |
| <Const\_>AR_Radau_Chain<\_Ext><Plus> | 15       | adaptive/constant | double/long double/...     | AR Gauss-Radau Chain method with*                          |
| ABITS<\_Plus>                        | adaptive | adaptive          | mpreal/mpreal_k            | Arbitrary precision method with*                           |
| AR_ABITS<\_Plus>                     | adaptive | adaptive          | mpreal/mpreal_k            | Algorithmic regularized arbitrary precision method with*   |