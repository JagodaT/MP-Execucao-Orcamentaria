Execução Orçamentária: Análise do Empenho
================

Inclusão de pacotes

``` r
library(tidyverse)
library(fs)
```

Checa tamanho dos arquivos

``` r
dir_info("data") %>% select(path,size,modification_time)
```

    ## # A tibble: 5 x 3
    ##   path                                 size modification_time  
    ##   <fs::path>                    <fs::bytes> <dttm>             
    ## 1 data/despesa.zip                  125.32M 2019-03-29 07:40:36
    ## 2 data/despesa2018.csv              301.04M 2019-03-29 18:05:13
    ## 3 data/despesa2018.zip               17.98M 2019-03-29 16:40:21
    ## 4 data/despesa2018_squished.csv      49.34M 2019-03-29 18:06:40
    ## 5 data/despesa2018_squished.zip       9.08M 2019-03-29 19:13:25

# Leitura de Dados Higienizados

Colocar num data frame (“tibble”). Nota: arquivo agora ’e UTF-8

``` r
fname_2018_squished_zip <- "data/despesa2018_squished.zip"
df_orcamento <- read_delim(fname_2018_squished_zip,delim=";",
                           quote="'") # evita erro com "
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   Poder = col_double(),
    ##   Grupo = col_double(),
    ##   `Nome Grupo` = col_logical(),
    ##   `Modalidade de Aplicação` = col_double(),
    ##   Elemento = col_double(),
    ##   `Sub Elemento` = col_double(),
    ##   Empenho = col_double(),
    ##   `Valor Empenhado` = col_number(),
    ##   `Valor Liquidado` = col_number(),
    ##   `Valor Pago` = col_number()
    ## )

    ## See spec(...) for full column specifications.

``` r
df_orcamento
```

    ## # A tibble: 99,362 x 32
    ##    Poder `Nome Poder` Grupo `Nome Grupo` `Modalidade de ~ `Nome Modalidad~
    ##    <dbl> <chr>        <dbl> <lgl>                   <dbl> <chr>           
    ##  1     1 Executivo        3 NA                         90 Aplicações Dire~
    ##  2     1 Executivo        3 NA                         90 Aplicações Dire~
    ##  3     1 Executivo        3 NA                         90 Aplicações Dire~
    ##  4     1 Executivo        3 NA                         90 Aplicações Dire~
    ##  5     1 Executivo        3 NA                         90 Aplicações Dire~
    ##  6     1 Executivo        3 NA                         90 Aplicações Dire~
    ##  7     1 Executivo        3 NA                         90 Aplicações Dire~
    ##  8     1 Executivo        3 NA                         90 Aplicações Dire~
    ##  9     1 Executivo        3 NA                         90 Aplicações Dire~
    ## 10     1 Executivo        3 NA                         90 Aplicações Dire~
    ## # ... with 99,352 more rows, and 26 more variables: Elemento <dbl>, `Nome
    ## #   Elemento` <chr>, `Sub Elemento` <dbl>, `Nome Sub Elemento` <chr>,
    ## #   Órgão <chr>, `Nome Órgão` <chr>, UO <chr>, `Nome UO` <chr>, UG <chr>,
    ## #   `Nome UG` <chr>, Credor <chr>, `Nome Credor` <chr>, `Fonte de
    ## #   Recursos` <chr>, `Nome Fonte de Recursos` <chr>, Processo <chr>,
    ## #   Função <chr>, `Nome Função` <chr>, `Sub Função` <chr>, `Nome Sub
    ## #   Função` <chr>, Licitação <chr>, `Nome Licitação` <chr>, Empenho <dbl>,
    ## #   Histórico <chr>, `Valor Empenhado` <dbl>, `Valor Liquidado` <dbl>,
    ## #   `Valor Pago` <dbl>

``` r
problems(df_orcamento)
```

    ## # tibble [0 x 4]
    ## # ... with 4 variables: row <int>, col <int>, expected <chr>, actual <chr>

Quantas linhas, colunas, ou dimensões?

``` r
nrow(df_orcamento)
```

    ## [1] 99362

``` r
ncol(df_orcamento)
```

    ## [1] 32

``` r
dim(df_orcamento)
```

    ## [1] 99362    32

Examinando data frame verticalmente

``` r
glimpse(df_orcamento)
```

    ## Observations: 99,362
    ## Variables: 32
    ## $ Poder                          <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1...
    ## $ `Nome Poder`                   <chr> "Executivo", "Executivo", "Exec...
    ## $ Grupo                          <dbl> 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3...
    ## $ `Nome Grupo`                   <lgl> NA, NA, NA, NA, NA, NA, NA, NA,...
    ## $ `Modalidade de Aplicação`      <dbl> 90, 90, 90, 90, 90, 90, 90, 90,...
    ## $ `Nome Modalidade de Aplicação` <chr> "Aplicações Diretas", "Aplicaçõ...
    ## $ Elemento                       <dbl> 339036, 339014, 339039, 339030,...
    ## $ `Nome Elemento`                <chr> "Outros Serviços de Terceiros -...
    ## $ `Sub Elemento`                 <dbl> NA, NA, NA, NA, NA, NA, NA, NA,...
    ## $ `Nome Sub Elemento`            <chr> NA, NA, NA, NA, NA, NA, NA, NA,...
    ## $ Órgão                          <chr> "21", "31", "29", "21", "40", "...
    ## $ `Nome Órgão`                   <chr> "Secretaria de Estado da Casa C...
    ## $ UO                             <chr> "2133", "3131", "2961", "2104",...
    ## $ `Nome UO`                      <chr> "Departamento de Trânsito do Es...
    ## $ UG                             <chr> "263100", "053100", "296100", "...
    ## $ `Nome UG`                      <chr> "DEPARTAMENTO DE TRANSITO DO RI...
    ## $ Credor                         <chr> "965.197.977-15", "042.670.427-...
    ## $ `Nome Credor`                  <chr> "ALEXANDRE CESAR DE SOUZA", "LU...
    ## $ `Fonte de Recursos`            <chr> "32", "12", "22", "00", "00", "...
    ## $ `Nome Fonte de Recursos`       <chr> "Taxas pelo Exercício do Poder ...
    ## $ Processo                       <chr> "E-12/061/7244/20", "E-12/171/1...
    ## $ Função                         <chr> "06", "22", "10", "04", "13", "...
    ## $ `Nome Função`                  <chr> "Segurança Pública", "Indústria...
    ## $ `Sub Função`                   <chr> "122", "122", "302", "122", "12...
    ## $ `Nome Sub Função`              <chr> "Administração Geral", "Adminis...
    ## $ Licitação                      <chr> "05", "07", "09", "09", "05", "...
    ## $ `Nome Licitação`               <chr> "DISPENSA", "NAO APLICAVEL", "P...
    ## $ Empenho                        <dbl> 4390, 1354, 7927, 561, 752, 140...
    ## $ Histórico                      <chr> "Cancelamento conforme Decreto ...
    ## $ `Valor Empenhado`              <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ `Valor Liquidado`              <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ `Valor Pago`                   <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...

# Análise do “Empenho”

Calcula sumarização de colunas que parecem conter valores:

``` r
summary(df_orcamento%>%select(contains("empenh"),
                              contains("valor")))
```

    ##     Empenho      Valor Empenhado     Valor Liquidado    
    ##  Min.   :    1   Min.   :0.000e+00   Min.   :0.000e+00  
    ##  1st Qu.:  236   1st Qu.:0.000e+00   1st Qu.:0.000e+00  
    ##  Median :  659   Median :2.588e+03   Median :1.254e+03  
    ##  Mean   : 2475   Mean   :3.747e+07   Mean   :3.768e+07  
    ##  3rd Qu.: 2085   3rd Qu.:9.188e+04   3rd Qu.:7.438e+04  
    ##  Max.   :27484   Max.   :1.930e+11   Max.   :1.930e+11  
    ##    Valor Pago       
    ##  Min.   :0.000e+00  
    ##  1st Qu.:0.000e+00  
    ##  Median :2.750e+02  
    ##  Mean   :3.315e+07  
    ##  3rd Qu.:4.694e+04  
    ##  Max.   :1.930e+11

Quantas linhas não preenchidas (com ‘NA’)?

``` r
df_orcamento$Empenho %>% is.na %>% sum
```

    ## [1] 0

Plota histograma do Empenho

``` r
df_orcamento %>% ggplot(aes(Empenho)) +
  geom_histogram(bins=30,fill="blue",color="black")
```

![](empenho_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Aplica “log” no valor:

``` r
df_orcamento %>% ggplot(aes(Empenho)) +
  geom_histogram(bins=30,fill="blue",color="black") +
  scale_x_log10()
```

![](empenho_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Estudo dos Órgãos

``` r
df_orcamento %>% count(`Órgão`,`Nome Órgão`)
```

    ## # A tibble: 27 x 3
    ##    Órgão `Nome Órgão`                                      n
    ##    <chr> <chr>                                         <int>
    ##  1 01    Assembléia Legislativa                          730
    ##  2 02    Tribunal de Contas do Estado do Rio de Janeir  1385
    ##  3 03    Tribunal de Justiça do Estado do Rio de Janei  2436
    ##  4 07    Secretaria de Estado de Obras                  4671
    ##  5 08    Vice-Governadoria                                93
    ##  6 09    Procuradoria Geral do Estado                   1636
    ##  7 10    Ministério Público                             2629
    ##  8 11    Defensoria Pública Geral do Estado             1163
    ##  9 13    Secretaria de Estado de Agricultura Pecuária   4800
    ## 10 14    Secretaria de Estado de Governo                 678
    ## # ... with 17 more rows

Ordenando pelo mais frequente

``` r
df_orcamento %>% count(`Órgão`,`Nome Órgão`,sort=T)
```

    ## # A tibble: 27 x 3
    ##    Órgão `Nome Órgão`                                      n
    ##    <chr> <chr>                                         <int>
    ##  1 18    Secretaria de Estado de Educação              19940
    ##  2 40    Secretaria de Estado de Ciência Tecnologia    12816
    ##  3 29    Secretaria de Estado de Saúde                 11056
    ##  4 21    Secretaria de Estado da Casa Civil e Desenvol 10603
    ##  5 26    Secretaria de Estado de Segurança              5542
    ##  6 13    Secretaria de Estado de Agricultura Pecuária   4800
    ##  7 07    Secretaria de Estado de Obras                  4671
    ##  8 20    Secretaria de Estado de Fazenda e Planejament  4192
    ##  9 31    Secretaria de Estado de Transportes            3436
    ## 10 10    Ministério Público                             2629
    ## # ... with 17 more rows

Empenho médio por órgão:

``` r
df_orcamento %>%
  group_by(`Nome Órgão`) %>%
  summarize(n=n(),medio=mean(Empenho),mediano=median(Empenho),desvio_padrao=sd(Empenho)) %>%
  arrange(-medio)
```

    ## # A tibble: 27 x 5
    ##    `Nome Órgão`                               n medio mediano desvio_padrao
    ##    <chr>                                  <int> <dbl>   <dbl>         <dbl>
    ##  1 Secretaria de Estado de Educação       19940 8837.   8546.         6089.
    ##  2 Secretaria de Estado de Saúde          11056 2257.   1668.         2086.
    ##  3 Secretaria de Estado de Ciência Tecno~  1062 1468.   1148.          891.
    ##  4 Ministério Público                      2629 1243.   1231           776.
    ##  5 Secretaria de Estado de Obras           4671  862.    539           800.
    ##  6 Secretaria de Estado da Casa Civil e ~ 10603  800.    413          1013.
    ##  7 Secretaria de Estado de Segurança       5542  795.    623           640.
    ##  8 Secretaria de Estado do Ambiente        2565  773.    670           638.
    ##  9 Secretaria de Estado de Ciência Tecno~ 12816  714.    483           712.
    ## 10 Tribunal de Justiça do Estado do Rio ~  2436  607.    520.          455.
    ## # ... with 17 more rows

Mostra como boxbplot

``` r
df_orcamento %>%
  rename(orgao=`Nome Órgão`) %>%
  mutate(orgao=orgao%>%fct_reorder(-Empenho)) %>%
  filter(as.integer(orgao)<6) %>%
  mutate(orgao=orgao%>%fct_rev) %>%
  ggplot(aes(orgao,Empenho)) +
  geom_boxplot(aes(fill=orgao)) +
  #scale_y_log10(trans="reverse") +
  coord_flip() +
  scale_y_continuous(trans="log10") +
  labs(title="Maiores Empenhos Medianos por Órgão",
       subtitle="Ano 2018") +
  theme(legend.position = "none",
        axis.title.y=element_blank())
```

![](empenho_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->