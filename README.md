# Implied Correlation of CDO Index Tranche


The implied correlations for the CDO index tranches are the correlations backed out by the market quoted prices using CDO valuation models available in the credit library, namely, the Poisson model and the Normal Copula model.  

Because there is no explicit solution for the implied correlation and the Monte Carlo simulation is employed to solve this inverse problem, it will be time consuming to run a root finding procedure by iteration. Hence a linear interpolation approximation is used instead. 

The model serves the purpose of calculating the implied correlations for standard index collateral debt obligation (CDO) tranches. In the market the credit default swaps (CDS) indexes CDX and Trac-X portfolios have been introduced and the standard tranches linked to these reference sets have also started to be actively quoted. Given the market price of each tranche, which is either quoted as a b/e spread or a 5% fee payment plus an upfront payment, we could back out an implied correlation value using CDO valuation models. 

Because there is no explicit solution for correlation and we employ Monte Carlo simulation (MC) for CDO valuation in model, it is very time consuming to run root finding by iteration. It is also impossible to get an accurate solution due to the presence of MC noise. Hence in the submitted model each tranche is first valued at various constant correlations ranging from 0 to 1 with interval 0.05. The implied correlation for each tranche is then found through linear interpolation by matching the calculated value with the market price.

The main approximation of this methodology is the linear interpolation. As shown in the next section, the tranche value is not a linear function of correlation, especially for mezzanine tranches. Therefore, a non-linearity error will occur.  However, this error could be reduced with increased granularity. Hence the main target is to assess if the correlation change interval (0.05 in the submitted model) is adequate.

Note that the implied correlation is model dependent. At the present time, there are two valuation models within credit derivative modeling framework, known as the Poisson model and the Normal Copula model (see https://finpricing.com/lib/EqBarrier.html).

It has been known that there exist duel solutions of the implied correlation for the mezzanine tranches. In the submitted model, the lower of the two correlations is being used in all cases, which is consistent with market practice. However, this will cause a problem when the implied correlation is used to find the correlation information of a non-standard tranche. A more reasonable approach to avoid dual solutions, namely base correlation methodology, has been proposed and quickly become a market standard.

In order to test the approximation of linear interpolation, we run a root find procedure as a benchmark to calculate the implied correlation.

The testing was conducted by implementing a test bisection root finding procedure in SH3 FirstLoss pricing template, same as the one used by the submitted model. SH3, which is the pricing engine of the credit derivatives. We then ran the submitted model and the test model at the same time and the calculated results were compared. The influence of MC noise on the implied correlation was also tested.

The market quotations, trade parameters, and credit spread curves of each reference name of the collateral pool of CDX4 as of April 20, 2005 are given in Appendix. 

Figures 1(a) and 1(b) illustrate how the value of a tranche (upfront fee for the 0~3% tranche and B/E spreads for other tranches) changes versus correlation. It can be seen that they are nonlinear, especially for the mezzanine tranches. Hence we should be careful when we apply linear interpolation approximation.

The implied correlations for each tranche, using submitted linear interpolation and root finding procedure, respectively, are shown in Table 1.   The discrepancies are quite small except for 3~7% tranche. Because the implied correlation for this tranche is small and have the same order as the interval, the discrepancies are relatively large. However, the absolute difference is still small and acceptable. As another test we change the linear interpolation interval from 0.2 to 0.01. Figure 2 shows the discrepancies of the implied correlations calculated using bid prices and Poisson model vs interpolation intervals. It confirms our expectation that the discrepancies converge as the interval decreases and the current choice of 0.05 is acceptable.

The effect of MC noise is tested by computing the implied correlation with different random seeds.  Using Poisson model, the implied correlations of 0~3% and 3~7% tranches are calculated and shown in Table 2.  The number of random path is 100,000. It is shown that the deviation is acceptable, given the fact that we are solving a reverse problem using MC simulation. Note that the rough estimations of the deviation incurred by the MC noise shown in Table 2 (3.1781% for 0~3% tranche and 14.0227% for 3~7% tranche) are larger than the discrepancies between the implied correlations using linear interpolation approximation (1.5751% for 0~3% tranche and 7.8894% for 3~7% tranche. 



 

 

 


