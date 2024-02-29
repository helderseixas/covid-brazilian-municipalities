# Analyzing the COVID-19 time-varying epidemiological parameters for large Brazilian municipalities using a model with fuzzy transitions between epidemic periods

## Supplementary material

This page provides supplementary information, data, and code regarding the paper titled "Analyzing the Time-Varying Epidemiological Parameters of COVID-19 in Large Brazilian Municipalities Using a Model with Fuzzy Transitions Between Epidemic Periods," submitted to the 2024 Simpósio Brasileiro de Computação Aplicada à Saúde (SBCAS) conference.

### Supplementary information

#### Municipality sample

Our dataset comprises the 41 largest Brazilian municipalities with populations exceeding 500,000: Aparecida de Goiânia/GO, Aracaju/SE, Belo Horizonte/MG, Belém/PA, Brasília/DF, Campinas/SP, Campo Grande/MS, Contagem/MG, Cuiabá/MT, Curitiba/PR, Duque de Caxias/RJ, Feira de Santana/BA, Florianópolis/SC, Fortaleza/CE, Goiânia/GO, Guarulhos/SP, Jaboatão dos Guararapes/PE, Joinville/SC, João Pessoa/PB, Juiz de Fora/MG, Londrina/PR, Maceió/AL, Manaus/AM, Natal/RN, Nova Iguaçu/RJ, Osasco/SP, Porto Alegre/RS, Recife/PE, Ribeirão Preto/SP, Rio de Janeiro/RJ, Salvador/BA, Santo André/SP, Serra/ES, Sorocaba/SP, São Bernardo do Campo/SP, São Gonçalo/RJ, São José dos Campos/SP, São Luís/MA, São Paulo/SP, Teresina/PI, and Uberlândia/MG.

#### Estimating *effective reproduction number* ($R_{t}$)

The *effective reproduction number* ($R_{t}$) is an essential indicator in epidemic surveillance, offering insights into whether a disease is spreading or diminishing. Values exceeding one signal ongoing transmission, while those below one indicate a decline in transmission. In our examination of COVID-19 within Brazilian municipalities, $R_{t}$ was computed using the time series of new cases sourced from SARS patients [1]. This computation utilized the *epyestim* library (https://github.com/lo-hfk/epyestim), a Python toolkit implementing the methodology proposed by Cori et al. [2].

The application of the *epyestim* method began with specifying the distribution for the Covid-19 generation time, approximating it based on the serial interval reported by Bi et al. [3] as $\sim \text{Gamma}(2.29, 0.36)$, with an average duration of 6.36 days. Furthermore, the delay between infection and symptom onset, representing the incubation period, was defined as $\sim \text{Lognormal}(1.57, 0.42)$, with an average time of 5.93 days [3]. Other parameters configured in *epyestim* included a 28-day window size to smooth the cases time series, a 14-day window size for the final rolling average, one hundred bootstrap samples for estimating $R_{t}$, and a prior $R_{t}$ $\sim \text{Gamma}(9.90, 9.28)$.

#### Parameter optimization

We utilized the stochastic differential evolution algorithm [4] in the Python Scipy library to optimize the model parameters. We configured this algorithm with specific parameters for optimization. These parameters included a maximum of 10,000 generations and a multiplier factor of five, ensuring that the total population size was five times the quantity of model parameters. Additionally, we employed an update strategy for the best solution vector once per generation, utilized the mutation strategy 'best1bin', and set a relative tolerance parameter of 0.01. For stringent convergence, we applied an absolute tolerance parameter of 0. We defined the mutation parameter range as (0.5, 1), with a recombination parameter 0.7. We initialized the algorithm using the Latin hypercube strategy and implemented an additional local optimization step after the global optimization process, employing the L-BFGS-B method.

#### Cross-correlation analysis

We conducted a cross-correlation analysis for each municipality between $\Delta_{H}$ and $R_{0}$. The cross-correlation coefficient $C_{k}(x,y)$, given by the equation below, was computed, where $x_{t}$ and $y_{t}$ denote the time series, and $k$ represents the lag [5].

$$
C_{k}(x,y) = \frac{\sum(x_{t+k} - \overline{x})(y_{t}-\overline{y})}{\sqrt{\sum(x_{t} - \overline{x})^{2}} \sqrt{\sum(y_{t} - \overline{y})^{2}} }
$$

To establish the significance limits for cross-correlation,  we introduced adjusted factors to mitigate spurious correlations resulting from autocorrelated time series, as suggested by Dean and Dunsmuir [6]. We calculated the weighted cross-correlation significance limits using the equation below, where $a$ and $b$ denote the autocorrelation coefficients at lag 1 for $x_{t}$ and $y_{t}$, respectively.

$$
\pm\frac{1.96}{\sqrt{n}}\sqrt{\frac{1+ab}{1-ab}}
$$

Before the cross-correlation analysis, we conducted stationarity transformations for the time series $\Delta_{H}$ and $R_{0}$ of the municipalities. We applied a 7-day differencing to generate a new time series denoted as $\Delta_{H}^{'}$ and $R_{0}^{'}$. Additionally, $\Delta_{H}^{'}$ was prewhitened using an ARIMA model of order (28, 0, 7), yielding $\Delta_{H}^{''}$.

We assessed the stationarity of the transformed time series using the Augmented Dickey-Fuller (ADF) test [7]. Furthermore, the Ljung–Box test [8] at lag one was conducted to verify that despite prewhitening, $\Delta_{H}^{''}$ remained non-white noise.

Finally, for each municipality and specific year, we computed cross-correlation analyses $C_{k}(\Delta_{H}^{''}, R_{0}^{'})$. Only significant $C_{k}(x,y)$ values were considered. The _xcorr_ method from the Python library _matplotlib.pyplot_ was utilized for estimating sample cross-correlation coefficients, while the ADF test and Ljung–Box test were performed using the Python library _statsmodels_.

## Code

## References
[1] DATASUS. Banco de Dados de Síndrome Respiratória Aguda Grave - incluindo dados da COVID-19. Database: Gov.BR [Internet]. Available from: https://opendatasus.saude.gov.br. Accessed: 2023 November 23. 2022.

[2] Anne Cori, Neil M Ferguson, Christophe Fraser, and Simon Cauchemez. "A new framework and software to estimate time-varying reproduction numbers during epidemics." *American Journal of Epidemiology* 178, no. 9 (2013): 1505-1512.

[3] Qifang Bi, Yongsheng Wu, Shujiang Mei, Chenfei Ye, Xuan Zou, Zhen Zhang, Xiaojian Liu, Lan Wei, Shaun A Truelove, Tong Zhang, and others. "Epidemiology and transmission of COVID-19 in 391 cases and 1286 of their close contacts in Shenzhen, China: a retrospective cohort study." *The Lancet Infectious Diseases* 20, no. 8 (2020): 911-919.

[4] Storn, R., & Price, K. (1997). Differential evolution-a simple and efficient heuristic for global optimization over continuous spaces. *Journal of Global Optimization*, 11(4), 341.

[5]: Shumway, R. H., Stoffer, D. S., & Stoffer, D. S. (2000). *Time Series Analysis and Its Applications* (Vol. 3). Springer.

[6]: Dean, R. T., & Dunsmuir, W. T. M. (2016). Dangers and uses of cross-correlation in analyzing time series in perception, performance, movement, and neuroscience: The importance of constructing transfer function autoregressive models. Behavior Research Methods, 48, 783–802.

[7]: Dickey, D. A., & Fuller, W. A. (1979). Distribution of the estimators for autoregressive time series with a unit root. *Journal of the American Statistical Association*, 74(366a), 427–431.

[8]: Ljung, G. M., & Box, G. E. P. (1978). On a measure of lack of fit in time series models. Biometrika, 65(2), 297–303.
