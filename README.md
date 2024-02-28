# Analyzing the COVID-19 time-varying epidemiological parameters for large Brazilian municipalities using a model with fuzzy transitions between epidemic periods

This text offers supplementary details regarding the paper titled "Analyzing the Time-Varying Epidemiological Parameters of COVID-19 in Large Brazilian Municipalities Using a Model with Fuzzy Transitions Between Epidemic Periods," submitted to the 2024 Simpósio Brasileiro de Computação Aplicada à Saúde (SBCAS) conference.

## Municipality Sample

Our dataset comprises the 41 largest Brazilian municipalities with populations exceeding 500,000: Aparecida de Goiânia/GO, Aracaju/SE, Belo Horizonte/MG, Belém/PA, Brasília/DF, Campinas/SP, Campo Grande/MS, Contagem/MG, Cuiabá/MT, Curitiba/PR, Duque de Caxias/RJ, Feira de Santana/BA, Florianópolis/SC, Fortaleza/CE, Goiânia/GO, Guarulhos/SP, Jaboatão dos Guararapes/PE, Joinville/SC, João Pessoa/PB, Juiz de Fora/MG, Londrina/PR, Maceió/AL, Manaus/AM, Natal/RN, Nova Iguaçu/RJ, Osasco/SP, Porto Alegre/RS, Recife/PE, Ribeirão Preto/SP, Rio de Janeiro/RJ, Salvador/BA, Santo André/SP, Serra/ES, Sorocaba/SP, São Bernardo do Campo/SP, São Gonçalo/RJ, São José dos Campos/SP, São Luís/MA, São Paulo/SP, Teresina/PI, and Uberlândia/MG.

## Estimating *effective reproduction number* ($R_{t}$)

The *effective reproduction number* ($R_{t}$) is an essential indicator in epidemic surveillance, offering insights into whether a disease is spreading or diminishing. Values exceeding one signal ongoing transmission, while those below one indicate a decline in transmission. In our examination of COVID-19 within Brazilian municipalities, $R_{t}$ was computed using the time series of new cases sourced from SARS patients [1]. This computation utilized the *epyestim* library (https://github.com/lo-hfk/epyestim), a Python toolkit implementing the methodology proposed by Cori et al. [2].

The application of the *epyestim* method began with specifying the distribution for the Covid-19 generation time, approximating it based on the serial interval reported by Bi et al. [3] as $\sim \text{Gamma}(2.29, 0.36)$, with an average duration of 6.36 days. Furthermore, the delay between infection and symptom onset, representing the incubation period, was defined as $\sim \text{Lognormal}(1.57, 0.42)$, with an average time of 5.93 days [3]. Other parameters configured in *epyestim* included a 28-day window size to smooth the cases time series, a 14-day window size for the final rolling average, one hundred bootstrap samples for estimating $R_{t}$, and a prior $R_{t}$ $\sim \text{Gamma}(9.90, 9.28)$.

## References
[1] DATASUS. Banco de Dados de Síndrome Respiratória Aguda Grave - incluindo dados da COVID-19. Database: Gov.BR [Internet]. Available from: https://opendatasus.saude.gov.br. Accessed: 2023 November 23. 2022.

[2] Anne Cori, Neil M Ferguson, Christophe Fraser, and Simon Cauchemez. "A new framework and software to estimate time-varying reproduction numbers during epidemics." *American Journal of Epidemiology* 178, no. 9 (2013): 1505-1512.

[3] Qifang Bi, Yongsheng Wu, Shujiang Mei, Chenfei Ye, Xuan Zou, Zhen Zhang, Xiaojian Liu, Lan Wei, Shaun A Truelove, Tong Zhang, and others. "Epidemiology and transmission of COVID-19 in 391 cases and 1286 of their close contacts in Shenzhen, China: a retrospective cohort study." *The Lancet Infectious Diseases* 20, no. 8 (2020): 911-919.
