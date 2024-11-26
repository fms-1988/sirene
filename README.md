## CO2e Emission Coefficients for the Brazilian Economy

The **Sirene** package is crafted to provide CO2e emissions data for various Brazilian economic activities. This package facilitates the estimation of direct emission coefficients for each sector, representing the volume of emissions in Gg relative to the production volume (expressed in aggregate value in millions of reais).

Beyond direct coefficients, it also computes the indirect emission coefficients. The estimation methodology is adopted from Luis Masa's paper, titled ["AN ESTIMATION OF THE CARBON FOOTPRINT IN SPANISH CREDIT INSTITUTIONS’ BUSINESS LENDING PORTFOLIO"](https://repositorio.bde.es/bitstream/123456789/29610/4/do2220e.pdf).

The **Sirene** package contains a series of variables that detail emissions from various sectors and other related data. The available variables are:

- `atividade_tru68_ibge`: Refers to activity (i) of TRU68.
- `production_values_mi_brl`: Represents the total production of activity (i) for the year (t) in millions of BRL.
- `ano`: Reference year (complete data available only between 2012 and 2020).
- `energia_Gg_CO2e_GWP_SAR`: Emissions of CO2e from the energy sector in Gg, calculated using the CO2e_GWP_SAR methodology.
- `residuo_Gg_CO2e_GWP_SAR`: Emissions of CO2e from the waste sector in Gg.
- `agropecuaria_Gg_CO2e_GWP_SAR`: Emissions of CO2e from the agricultural sector in Gg.
- `ippu_Gg_CO2e_GWP_SAR`: Emissions of CO2e from the IPPU sector in Gg.
- `lulucf_Gg_CO2e_GWP_SAR`: Emissions of CO2e from the LULUCF sector in Gg.
- `total_Gg_CO2e_GWP_SAR`: Total CO2e emissions in Gg.
- `active_loan_portfolio_mi_brl`: Value of loans by sector in millions of BRL.
- `q_direct`: Coefficient of direct emissions.
- `q_total`: Coefficient of total emissions.
- `q_indirect`: Coefficient of indirect emissions.


### Instalation

```python
!pip install sirene
from sirene import srn as srn
coef67_t = srn.coef('2019', lulucf = False).result
```

### Reading the Raw Emission Data (MCTI/SIRENE)

Emission data is available for 3 different gases ('CO2', 'CH4', 'N2O'). If preferred, it is also possible to consult equivalent emissions ('CO2e_GWP_SAR', 'CO2e_GWP_AR5', 'CO2e_GTP_AR5'). The available sectors are: 'agropecuaria', 'energia', 'ippu', 'lulucf', 'residuos' and 'total-brasil-1'.

```python
res_m = srn.read('residuos','CO2')
```

### Main Function: coef()

The `coef()` function is the core of this package and returns all of the above-listed variables. The arguments it accepts are:

- `ano`: String representing the desired year, with values between 2012 and 2020.
- `household`: Boolean (True or False) determining whether households should be considered when calculating emissions and coefficients.
- `emission`: Defines which type of emission will be considered when constructing the emission coefficients. Available options are: 'total', 'total_no_lulucf', 'lulucf', 'ippu', 'residuo', and 'agropecuaria'.

**Usage Example**:

```python
result_t = srn.coef('2019', emission='lulucf', household=False).result
```

---



```python
# !pip install sirene==0.0.19 #até a versão 0.0.18 eu usava o saldo da carteira de dezembro, agora eu uso a média anual.
#from sirene import srn as srn
from sirene import srn_defl as srn #(dados tru deflacionados)
import matplotlib.pyplot as plt
import pandas as pd
```

    /home/felipe/.local/lib/python3.10/site-packages/matplotlib/projections/__init__.py:63: UserWarning: Unable to import Axes3D. This may be due to multiple versions of Matplotlib being installed (e.g. as a system package and as a pip package). As a result, the 3D projection is not available.
      warnings.warn("Unable to import Axes3D. This may be due to multiple versions of "


A aplicação Sirene tem duas finalidade: 
1) facilitar o acesso aos dados brutos de emissões de Gases do efeito etufa gerados pelo MCTI.
2) acessar os dados de emissões desagregados usando a metodologia de desagregação proposta no artigo tal. De forma secundária, essa aplicação também permite obter dados setoriais sobre valor bruto da produção e valor dos empéstimos por setor. Com os dados de emissão desagregados e o VBP podemos estimas os coeficientes de emissão total, direto eindireto segundo a metodologiaproposta por MAZA. Cabe lembrar que os dados tem perioditocade anual e, segundo o último relarório do SIRENE, estão disponíveis entre os asnos xxxx e yyyy.


```python
# 1) lendo os dados brutos de emissão
# Esses dados estão agregados em 5 setores ('agropecuaria', 'energia', 'ippu', 'lulucf', 'residuos') e diferentes subsetores. Para cada subsetor o MCTI estimas as emissões dos principais gases de efeito estufa: 'CO2', 'CH4', 'N2O'. Além disso, para uma visão mais agregada, podemos converter essas emissões em CO2e segundo diferentes metodologias: 'CO2e_GWP_SAR', 'CO2e_GWP_AR5', 'CO2e_GTP_AR5'.

# 1.1) retomando os dados de emissão de um setor específico:


srn.read('agropecuaria') # emissão CO2 entre 1990 e 2020 estimado pelo MCTI (6º edição)
srn.read('energia')
srn.read('ippu')
srn.read('lulucf')
srn.read('residuos') # emissão CO2 entre 1990 e 2020 estimado pelo MCTI (6º edição) para o setor residuos
# srn.read('total-brasil-1','CO2e_GWP_SAR')
'''---'''
```




    '---'




```python
#1.2) dados de emissão para diferentes gases

srn.read('agropecuaria','CO2') # emissão de CO2, em Gg anual, pelo setor 'agropecuaria' 
srn.read('agropecuaria','CH4') #
srn.read('agropecuaria','N2O') # 
'''---'''
```




    '---'




```python
# 1.3) agregando as emissões segundo metodologias diferentes

srn.read('agropecuaria','CO2e_GWP_SAR') # emissão de CO2, em Gg anual, pelo setor 'agropecuaria' 
srn.read('agropecuaria','CO2e_GWP_AR5') #
srn.read('agropecuaria','CO2e_GTP_AR5') # 
srn.read('total-brasil-1','CO2e_GWP_SAR')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>4</th>
      <th>1990.0</th>
      <th>1991.0</th>
      <th>1992.0</th>
      <th>1993.0</th>
      <th>1994.0</th>
      <th>1995.0</th>
      <th>1996.0</th>
      <th>1997.0</th>
      <th>1998.0</th>
      <th>1999.0</th>
      <th>...</th>
      <th>2011.0</th>
      <th>2012.0</th>
      <th>2013.0</th>
      <th>2014.0</th>
      <th>2015.0</th>
      <th>2016.0</th>
      <th>2017.0</th>
      <th>2018.0</th>
      <th>2019.0</th>
      <th>2020.0</th>
    </tr>
    <tr>
      <th>setor_nfr</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Total</th>
      <td>1.513050e+06</td>
      <td>1.350780e+06</td>
      <td>1.473045e+06</td>
      <td>1.524658e+06</td>
      <td>1.520309e+06</td>
      <td>2.658961e+06</td>
      <td>1.933597e+06</td>
      <td>1.628555e+06</td>
      <td>1.906803e+06</td>
      <td>1.876171e+06</td>
      <td>...</td>
      <td>1.330959e+06</td>
      <td>1.214650e+06</td>
      <td>1.465501e+06</td>
      <td>1.354190e+06</td>
      <td>1.461633e+06</td>
      <td>1.473392e+06</td>
      <td>1.363034e+06</td>
      <td>1.368710e+06</td>
      <td>1.602461e+06</td>
      <td>1.675759e+06</td>
    </tr>
    <tr>
      <th>1. Energia</th>
      <td>1.928096e+05</td>
      <td>1.963756e+05</td>
      <td>2.015303e+05</td>
      <td>2.060034e+05</td>
      <td>2.148438e+05</td>
      <td>2.309830e+05</td>
      <td>2.482115e+05</td>
      <td>2.643361e+05</td>
      <td>2.718498e+05</td>
      <td>2.815097e+05</td>
      <td>...</td>
      <td>3.879157e+05</td>
      <td>4.209064e+05</td>
      <td>4.523506e+05</td>
      <td>4.788814e+05</td>
      <td>4.544386e+05</td>
      <td>4.247321e+05</td>
      <td>4.295025e+05</td>
      <td>4.067935e+05</td>
      <td>4.083429e+05</td>
      <td>3.894841e+05</td>
    </tr>
    <tr>
      <th>2. Processos industriais e uso de produtos (IPPU)</th>
      <td>5.692059e+04</td>
      <td>6.199831e+04</td>
      <td>6.024298e+04</td>
      <td>6.320786e+04</td>
      <td>6.321482e+04</td>
      <td>6.801612e+04</td>
      <td>6.739034e+04</td>
      <td>7.045198e+04</td>
      <td>7.502388e+04</td>
      <td>7.437740e+04</td>
      <td>...</td>
      <td>9.387738e+04</td>
      <td>9.520686e+04</td>
      <td>9.679476e+04</td>
      <td>9.449125e+04</td>
      <td>9.542089e+04</td>
      <td>9.370133e+04</td>
      <td>9.816093e+04</td>
      <td>9.657526e+04</td>
      <td>1.014626e+05</td>
      <td>1.019364e+05</td>
    </tr>
    <tr>
      <th>3. Agropecuária</th>
      <td>3.296129e+05</td>
      <td>3.387376e+05</td>
      <td>3.439877e+05</td>
      <td>3.492618e+05</td>
      <td>3.568910e+05</td>
      <td>3.594707e+05</td>
      <td>3.388175e+05</td>
      <td>3.463707e+05</td>
      <td>3.521076e+05</td>
      <td>3.566238e+05</td>
      <td>...</td>
      <td>4.630810e+05</td>
      <td>4.620783e+05</td>
      <td>4.674368e+05</td>
      <td>4.728464e+05</td>
      <td>4.762785e+05</td>
      <td>4.871702e+05</td>
      <td>4.645018e+05</td>
      <td>4.641781e+05</td>
      <td>4.683711e+05</td>
      <td>4.776705e+05</td>
    </tr>
    <tr>
      <th>4. Uso da Terra, Mudança do Uso da Terra e Florestas (LULUCF)</th>
      <td>9.075130e+05</td>
      <td>7.258803e+05</td>
      <td>8.385865e+05</td>
      <td>8.753576e+05</td>
      <td>8.529330e+05</td>
      <td>1.966234e+06</td>
      <td>1.243209e+06</td>
      <td>9.102377e+05</td>
      <td>1.169334e+06</td>
      <td>1.123254e+06</td>
      <td>...</td>
      <td>3.285898e+05</td>
      <td>1.785544e+05</td>
      <td>3.873587e+05</td>
      <td>2.460640e+05</td>
      <td>3.726926e+05</td>
      <td>4.044966e+05</td>
      <td>3.064440e+05</td>
      <td>3.345022e+05</td>
      <td>5.568179e+05</td>
      <td>6.370386e+05</td>
    </tr>
    <tr>
      <th>5. Resíduos</th>
      <td>2.619352e+04</td>
      <td>2.778839e+04</td>
      <td>2.869725e+04</td>
      <td>3.082708e+04</td>
      <td>3.242630e+04</td>
      <td>3.425745e+04</td>
      <td>3.596863e+04</td>
      <td>3.715815e+04</td>
      <td>3.848735e+04</td>
      <td>4.040561e+04</td>
      <td>...</td>
      <td>5.749480e+04</td>
      <td>5.790412e+04</td>
      <td>6.156007e+04</td>
      <td>6.190650e+04</td>
      <td>6.280232e+04</td>
      <td>6.329223e+04</td>
      <td>6.442457e+04</td>
      <td>6.666138e+04</td>
      <td>6.746670e+04</td>
      <td>6.962985e+04</td>
    </tr>
  </tbody>
</table>
<p>6 rows × 31 columns</p>
</div>




```python
# 1.3) conhecendo os subsetores de um determinado setor

srn.read('energia').index

```




    Index(['1. Energia',
           '1.A.   Atividades de Queima de Combustíveis (abordagem setorial)',
           '1.A.1.     Indústrias de Energia',
           '1.A.1.a.       Produção de eletricidade e calor como atividade principal',
           '1.A.1.b.       Refino de petróleo',
           '1.A.1.c.       Produção de combustíveis sólidos e outras indústrias de energia',
           '1.A.2.     Indústrias de Transformação e Construção',
           '1.A.2.a.       Ferro e aço', '1.A.2.b.       Metais não ferrosos',
           '1.A.2.c.       Produtos quimicos',
           '1.A.2.d.       Celulose, papel e impressão',
           '1.A.2.e.       Processamento de alimentos, bebidas e tabaco',
           '1.A.2.f.       Minerais não metálicos',
           '1.A.2.g.       Equipamentos de transporte',
           '1.A.2.i.       Mineração (exceto combustíveis) e extração',
           '1.A.2.l.       Têxtil e couro', '1.A.3.     Transporte',
           '1.A.3.a.ii.          Aviação doméstica',
           '1.A.3.b.       Transporte rodoviário',
           '1.A.3.c.       Transporte Ferroviário',
           '1.A.3.d.ii.          Navegação doméstica',
           '1.A.3.e.       Outros transportes', '1.A.4.     Outros Setores',
           '1.A.4.a.       Comercial / institucional',
           '1.A.4.b.       Residencial',
           '1.A.4.c.       Agricultura / silvicultura / pesca / piscicultura',
           '1.A.5.     Não Especificado',
           '1.B.   Emissões Fugitivas a Partir da Produção de Combustíveis',
           '1.B.1.     Combustíveis sólidos', '1.B.2.     Petróleo e Gás Natural ',
           '1.C.   Transporte e armazenamento de CO2'],
          dtype='object', name='setor_nfr')




```python

# 1.4) retomando dados de um subsetor específico

srn.read('energia').loc['1.A.4.b.       Residencial',:][20:-1]
```




    4
    2010.0    17249.375951
    2011.0    17486.915522
    2012.0    17597.816326
    2013.0    17993.795005
    2014.0    18001.931522
    2015.0    18020.648453
    2016.0    18209.105762
    2017.0    18348.639738
    2018.0    18211.226425
    2019.0    18132.857115
    Name: 1.A.4.b.       Residencial, dtype: float64



# 2) lendo dados desagregados de emissão
Cada setor dos dados MCTI (agora chamados setores mcti) foram desagregados em 68 atividades (segundo a classificação do IBGE). Sendo assim, podemos acessar as emissõs de uma atividade específica para cada setor (que na verdade deveria se chamar origem das emissões. O MCTI chama de setor, nãofaz muito sentido).



```python

srn.co2e


#srn.scr.co2e



```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>year</th>
      <th>energia_Gg_CO2e_GWP_SAR</th>
      <th>residuo_Gg_CO2e_GWP_SAR</th>
      <th>agropecuaria_Gg_CO2e_GWP_SAR</th>
      <th>ippu_Gg_CO2e_GWP_SAR</th>
      <th>lulucf_Gg_CO2e_GWP_SAR</th>
      <th>total_Gg_CO2e_GWP_SAR</th>
      <th>atividade_tru68_ibge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0191\nAgricultura, inclusive o apoio à agricul...</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>118165.392346</td>
      <td>118165.392346</td>
      <td>0191</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0192\nPecuária, inclusive o apoio à pecuária</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>569.089500</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>786620.547583</td>
      <td>787189.637083</td>
      <td>0192</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0280\nProdução florestal; pesca e aquicultura</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2727.011575</td>
      <td>2727.011575</td>
      <td>0280</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1091\nAbate e produtos de carne, inclusive os ...</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>443.051700</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>443.051700</td>
      <td>1091</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1100\nFabricação de bebidas</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>81.847500</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>81.847500</td>
      <td>1100</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1574</th>
      <td>8692\nSaúde privada</td>
      <td>2020</td>
      <td>54.567948</td>
      <td>17.868048</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>72.435996</td>
      <td>8691 + 8692</td>
    </tr>
    <tr>
      <th>1575</th>
      <td>9080\nAtividades artísticas, criativas e de es...</td>
      <td>2020</td>
      <td>30.165292</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>30.165292</td>
      <td>9080</td>
    </tr>
    <tr>
      <th>1576</th>
      <td>9480\nOrganizações associativas e outros servi...</td>
      <td>2020</td>
      <td>91.784941</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>91.784941</td>
      <td>9480</td>
    </tr>
    <tr>
      <th>1577</th>
      <td>9700\nServiços domésticos</td>
      <td>2020</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>9700</td>
    </tr>
    <tr>
      <th>1578</th>
      <td>RESIDENCIAL</td>
      <td>2020</td>
      <td>25841.663219</td>
      <td>65263.650548</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>91105.313767</td>
      <td>RESIDENCIAL</td>
    </tr>
  </tbody>
</table>
<p>1579 rows × 9 columns</p>
</div>



Vale a pena resaltar que nem todos os anos foram desagregado, o motivo é que, para desagregar, precisamos de dados já desagregados sobre o VBP, o que não está disponível pelo IBGE antes de 20xx.



```python

# para dados que somam saude publica = saude privada e educação publica  educação privada

#coef(self, year, emission='total', household=True, ajust_sicor=True, reference_year='2011')
srn.co2e_67.head(10) # (8591 + 8592) e (8691 + 8692)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>atividade_tru68_ibge</th>
      <th>energia_Gg_CO2e_GWP_SAR</th>
      <th>residuo_Gg_CO2e_GWP_SAR</th>
      <th>agropecuaria_Gg_CO2e_GWP_SAR</th>
      <th>ippu_Gg_CO2e_GWP_SAR</th>
      <th>lulucf_Gg_CO2e_GWP_SAR</th>
      <th>total_Gg_CO2e_GWP_SAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1990</td>
      <td>0191</td>
      <td>0.0</td>
      <td>0.0000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>118165.392346</td>
      <td>118165.392346</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1990</td>
      <td>0192</td>
      <td>0.0</td>
      <td>569.0895</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>786620.547583</td>
      <td>787189.637083</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1990</td>
      <td>0280</td>
      <td>0.0</td>
      <td>0.0000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>2727.011575</td>
      <td>2727.011575</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1990</td>
      <td>1091</td>
      <td>0.0</td>
      <td>443.0517</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>443.051700</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1990</td>
      <td>1100</td>
      <td>0.0</td>
      <td>81.8475</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>81.847500</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1990</td>
      <td>1700</td>
      <td>0.0</td>
      <td>186.8790</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>186.879000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1990</td>
      <td>2091</td>
      <td>0.0</td>
      <td>0.0000</td>
      <td>0.0</td>
      <td>8744.469277</td>
      <td>0.000000</td>
      <td>8744.469277</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1990</td>
      <td>2300</td>
      <td>0.0</td>
      <td>0.0000</td>
      <td>0.0</td>
      <td>15170.640561</td>
      <td>0.000000</td>
      <td>15170.640561</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1990</td>
      <td>2491</td>
      <td>0.0</td>
      <td>0.0000</td>
      <td>0.0</td>
      <td>28232.024448</td>
      <td>0.000000</td>
      <td>28232.024448</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1990</td>
      <td>2492</td>
      <td>0.0</td>
      <td>0.0000</td>
      <td>0.0</td>
      <td>4123.240826</td>
      <td>0.000000</td>
      <td>4123.240826</td>
    </tr>
  </tbody>
</table>
</div>




```python

codigo: emissoes setor x1 atividade y
codigo: emissões setor x2 atividade y
.
.
.
codigo: total x1x2..

também é possível acessar os dados já agregados

codigo: emissões atividade y
```

# 3) abrangencia da decomposição
A desagregação dos dados de emissão do MCTI em 68 atividades não é algo trivial. Dada essa complexidade, nem toda emissão foi desagregada em alguma atividade. Sendo assim, se compararmos o total de emissões das 68 atividades com o total estimado pelo MCTI, veremos que o primeiro valor é menor. De toda forma, tomamos o cuidado para realizar o máximo de desagregações possíveis e sem muita perda de informação para garantir a confiabilidade dos dados.

gráfico desagregação.



```python
import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)


# 1) emission separated (68 activities)
E_total_68 = pd.DataFrame({'year':[],'total68_Gg_CO2e_GWP_SAR':[]})
for t in range(2010,2021):
  result_t = srn.coef(str(t)).result
  footprint = result_t['total_Gg_CO2e_GWP_SAR'].sum()
  df_ = pd.DataFrame({'year':[result_t.ano[0]],'total68_Gg_CO2e_GWP_SAR':[footprint]})
  E_total_68 = pd.concat([E_total_68,df_])

# 2) emission aggregated (official data from MCTI)
E_total = pd.DataFrame(srn.read('total-brasil-1','CO2e_GWP_SAR').loc['Total',:])
E_total.reset_index(inplace=True)
E_total.columns = ['year','total_Gg_CO2e_GWP_SAR']

# 3) plot
E = E_total.merge(E_total_68, on='year')

plt.figure(figsize=(6,4))
plt.plot(E['year'], E['total_Gg_CO2e_GWP_SAR'], label='Total emission (MCTI)', marker='o')
plt.plot(E['year'], E['total68_Gg_CO2e_GWP_SAR'], label='Total emission (MCTI_disaggregated)', marker='o')

plt.title('Year vs. Values')
plt.xlabel('Year')
plt.ylabel('Emission (Gg of CO2e_GWP_SAR)')
plt.legend()
plt.grid(True)
plt.show()
```


    
![png](sirene_apresentacao2_files/sirene_apresentacao2_13_0.png)
    


4) coeficientes de emissões
Usando os dados de emissões desagregados e o VPB deflacionado (veja patienne xxxx) podemos estimar os coeficientes de emissões por atividade (veja a petodologiaproposta por Masa). Para estimar esses coeficientes podemos considerar o setor household, que repesenta as famílias e são responsáveis por emissões do tipo 'queima de resíduos em zona rural'..... 


coeficeinte de emissão com household
coeficiente de emissão sem householld
coeficiente de emissão total
coeficiente de emissão primario
coeficiente de emissão secundário


```python
# meplor chamar de df ou result, não coef_t
#coef_t = srn.coef(year='2010', emission='total', household=True, ajust_sicor=True, reference_year='2011')
coef_t.reference_year
coef_t.ajust_sicor
coef_t.defl_df #importante
coef_t.defl_num # escalar
coef_t.emission # tipo de emissão: 'total' ....
coef_t.household # bolean true, false
coef_t.mLeontiefBarr.shape #matriz leontieff (67,67)
coef_t.result[['atividade_tru68_ibge', 'production_values_mi_brl', 'ano']] # dados IBGE
coef_t.result[['ano', 'production_values_mi_brl', 
               'total_Gg_CO2e_GWP_SAR', 'ano', 'q_direct', 'q_total', 'q_indirect']]  #coeficientes de emissão segundo maza

coef_t.result[['ano', 'production_values_mi_brl', 'total_Gg_CO2e_GWP_SAR', 'active_loan_portfolio_mi_brl']] # dados sobre emprestimos
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ano</th>
      <th>production_values_mi_brl</th>
      <th>total_Gg_CO2e_GWP_SAR</th>
      <th>active_loan_portfolio_mi_brl</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0191\nAgricultura, inclusive o apoio à agricultura e a pós-colheita</th>
      <td>2010</td>
      <td>183182.039144</td>
      <td>131607.728990</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0192\nPecuária, inclusive o apoio à pecuária</th>
      <td>2010</td>
      <td>90525.194109</td>
      <td>621796.010176</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0280\nProdução florestal; pesca e aquicultura</th>
      <td>2010</td>
      <td>22056.349423</td>
      <td>-28159.832875</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0580\nExtração de carvão mineral e de minerais não-metálicos</th>
      <td>2010</td>
      <td>16096.405309</td>
      <td>755.047780</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0680\nExtração de petróleo e gás, inclusive as atividades de apoio</th>
      <td>2010</td>
      <td>127280.714035</td>
      <td>1836.226776</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8691 + 8692\nSaúde</th>
      <td>2010</td>
      <td>236241.163018</td>
      <td>287.059913</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9080\nAtividades artísticas, criativas e de espetáculos</th>
      <td>2010</td>
      <td>25274.979599</td>
      <td>30.998257</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9480\nOrganizações associativas e outros serviços pessoais</th>
      <td>2010</td>
      <td>113122.863538</td>
      <td>147.791306</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9700\nServiços domésticos</th>
      <td>2010</td>
      <td>43754.711667</td>
      <td>0.000000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>\nRESIDENCIAL</th>
      <td>2010</td>
      <td>1755428.097204</td>
      <td>77766.816042</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>67 rows × 4 columns</p>
</div>




```python
coeficeinte de emissão com household
coeficiente de emissão sem householld
coeficiente de emissão total
coeficiente de emissão primario
coeficiente de emissão secundário
```


```python

```


```python

```


```python
#Atividades TRU67
#aude pública e privada estão agregadas (8691 + 8692)
#educação pública e privada estão agregadas (8591 + 8592)
len(srn.atividades_tru67)#[0:4]
```




    67




```python
#dados sobre emissões (dados Brutos do MCTI), (agregados por subsetores)
#subsetores: 'agropecuaria', 'energia', 'ippu', 'lulucf', 'residuos', 'total-brasil-1'
#ghg:   'CO2e_GWP_SAR', 'CO2e_GWP_AR5', 'CO2e_GTP_AR5', 'CO2', 'CH4', 'N2O'
srn.read(sector='agropecuaria',gas='CO2')[1:2]
```





  <div id="df-7b78581a-881a-4db7-ae24-a0a329cf94bd" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>4</th>
      <th>1990.0</th>
      <th>1991.0</th>
      <th>1992.0</th>
      <th>1993.0</th>
      <th>1994.0</th>
      <th>1995.0</th>
      <th>1996.0</th>
      <th>1997.0</th>
      <th>1998.0</th>
      <th>1999.0</th>
      <th>...</th>
      <th>2011.0</th>
      <th>2012.0</th>
      <th>2013.0</th>
      <th>2014.0</th>
      <th>2015.0</th>
      <th>2016.0</th>
      <th>2017.0</th>
      <th>2018.0</th>
      <th>2019.0</th>
      <th>2020.0</th>
    </tr>
    <tr>
      <th>setor_nfr</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3.A.   Fermentação Entérica</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 31 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-7b78581a-881a-4db7-ae24-a0a329cf94bd')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-7b78581a-881a-4db7-ae24-a0a329cf94bd button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-7b78581a-881a-4db7-ae24-a0a329cf94bd');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    </div>
  </div>





```python
#dados sobre emissões (desagregados por atividade TRU68)
srn.co2e
```





  <div id="df-8f4f405f-4dc7-45d9-9fde-6e221623ff06" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>year</th>
      <th>energia_Gg_CO2e_GWP_SAR</th>
      <th>residuo_Gg_CO2e_GWP_SAR</th>
      <th>agropecuaria_Gg_CO2e_GWP_SAR</th>
      <th>ippu_Gg_CO2e_GWP_SAR</th>
      <th>lulucf_Gg_CO2e_GWP_SAR</th>
      <th>total_Gg_CO2e_GWP_SAR</th>
      <th>atividade_tru68_ibge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0191\nAgricultura, inclusive o apoio à agricul...</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>118165.392346</td>
      <td>118165.392346</td>
      <td>0191</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0192\nPecuária, inclusive o apoio à pecuária</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>569.089500</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>786620.547583</td>
      <td>787189.637083</td>
      <td>0192</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0280\nProdução florestal; pesca e aquicultura</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2727.011575</td>
      <td>2727.011575</td>
      <td>0280</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1091\nAbate e produtos de carne, inclusive os ...</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>443.051700</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>443.051700</td>
      <td>1091</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1100\nFabricação de bebidas</td>
      <td>1990</td>
      <td>0.000000</td>
      <td>81.847500</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>81.847500</td>
      <td>1100</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1574</th>
      <td>8692\nSaúde privada</td>
      <td>2020</td>
      <td>54.567948</td>
      <td>17.868048</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>72.435996</td>
      <td>8691 + 8692</td>
    </tr>
    <tr>
      <th>1575</th>
      <td>9080\nAtividades artísticas, criativas e de es...</td>
      <td>2020</td>
      <td>30.165292</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>30.165292</td>
      <td>9080</td>
    </tr>
    <tr>
      <th>1576</th>
      <td>9480\nOrganizações associativas e outros servi...</td>
      <td>2020</td>
      <td>91.784941</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>91.784941</td>
      <td>9480</td>
    </tr>
    <tr>
      <th>1577</th>
      <td>9700\nServiços domésticos</td>
      <td>2020</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>9700</td>
    </tr>
    <tr>
      <th>1578</th>
      <td>RESIDENCIAL</td>
      <td>2020</td>
      <td>25841.663219</td>
      <td>65263.650548</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>91105.313767</td>
      <td>RESIDENCIAL</td>
    </tr>
  </tbody>
</table>
<p>1579 rows × 9 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-8f4f405f-4dc7-45d9-9fde-6e221623ff06')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-8f4f405f-4dc7-45d9-9fde-6e221623ff06 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-8f4f405f-4dc7-45d9-9fde-6e221623ff06');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-ce9fdaf6-c25e-4ec6-b195-bde1556db2ac">
  <button class="colab-df-quickchart" onclick="quickchart('df-ce9fdaf6-c25e-4ec6-b195-bde1556db2ac')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-ce9fdaf6-c25e-4ec6-b195-bde1556db2ac button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>





```python
#dados sobre crédito rural (2012 a 2023) (valores nomnais)
#srn.scr_67
srn.scr[0:2]
```





  <div id="df-26cecce0-d35b-41b4-997a-c65712bb3bec" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cliente</th>
      <th>cnae_subclasse</th>
      <th>ano</th>
      <th>cnae_subclasse_bcb</th>
      <th>codigo_cnae_ibge</th>
      <th>atividade_tru68_ibge</th>
      <th>sum_carteira_ativa</th>
      <th>base_dados</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>PF</td>
      <td>-</td>
      <td>2012</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>9.872885e+11</td>
      <td>scr</td>
    </tr>
    <tr>
      <th>1</th>
      <td>PF</td>
      <td>-</td>
      <td>2013</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>1.117671e+12</td>
      <td>scr</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-26cecce0-d35b-41b4-997a-c65712bb3bec')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-26cecce0-d35b-41b4-997a-c65712bb3bec button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-26cecce0-d35b-41b4-997a-c65712bb3bec');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-24f62589-ae2d-4010-8883-702ba1f019d2">
  <button class="colab-df-quickchart" onclick="quickchart('df-24f62589-ae2d-4010-8883-702ba1f019d2')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-24f62589-ae2d-4010-8883-702ba1f019d2 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>





```python
#ajust_sicor=True : considera dados do sicor ao construir os coeficientes de emissão, do contrario considera apenas os dados do SNC.
# Os dados do SNC já contemplam os dados do SICOR, o problema é que eles são tratados como pessoas físicas nesse caso (household0).
# Porém, o mais correto é tratar esses dados como fessoas jurídicas, dadas as particularidas do sistema produtivo agropecuário e seu financiamento.

#descrição dos resulltados
'''
atividade_tru68_ibge: Refers to activity (i) of TRUGS.
production_values_mi_br1: Represents the total production of activity (i) for the year (t) in millions of BRL.
ano: Reference year (complete data available only between 2012 and 2020).
energia_Gg_C02e_GWP_SAR: Emissions of CO2e from the energy sector in Gg, calculated using the C02e_GWP_SAR methodology.
residuo_Gg_C02e_GWP_SAR: Emissions of CO2e from the waste sector in Gg.
agropecuaria_Gg_C02e_GWP_SAR: Emissions of CO2e from the agricultural sector in Gg.
ippu_Gg_Co2e_GWP_SAR: Emissions of CO2e from the IPPU sector in Gg.
lulucf_Gg_C02e_GWP_SAR: Emissions of CO2e from the LULUCF sector in Gg.
total_Gg_CO2e_GWP_SAR: Total CO2e emissions in Gg.
active_loan_portfolio_mi_br: Value of loans by sector in millions of BRL.
q_direct: Coefficient of direct emissions.
q_total: Coefficient of total emissions.
q_indirect: Coefficient of indirect emissions.
'''

sys = srn.coef('2010', emission='total', household=True, ajust_sicor=True)
sys.result
```





  <div id="df-12ab0b96-7e75-4491-bd2d-683df9d6242f" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>atividade_tru68_ibge</th>
      <th>production_values_mi_brl</th>
      <th>ano</th>
      <th>energia_Gg_CO2e_GWP_SAR</th>
      <th>residuo_Gg_CO2e_GWP_SAR</th>
      <th>agropecuaria_Gg_CO2e_GWP_SAR</th>
      <th>ippu_Gg_CO2e_GWP_SAR</th>
      <th>lulucf_Gg_CO2e_GWP_SAR</th>
      <th>total_Gg_CO2e_GWP_SAR</th>
      <th>active_loan_portfolio_mi_brl</th>
      <th>q_direct</th>
      <th>q_total</th>
      <th>q_indirect</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0191\nAgricultura, inclusive o apoio à agricultura e a pós-colheita</th>
      <td>0191</td>
      <td>168861</td>
      <td>2010</td>
      <td>9835.322627</td>
      <td>0.000000</td>
      <td>72759.139369</td>
      <td>0.0</td>
      <td>49013.266995</td>
      <td>131607.728990</td>
      <td>NaN</td>
      <td>0.779385</td>
      <td>1.73401</td>
      <td>0.954625</td>
    </tr>
    <tr>
      <th>0192\nPecuária, inclusive o apoio à pecuária</th>
      <td>0192</td>
      <td>83448</td>
      <td>2010</td>
      <td>5140.015130</td>
      <td>1602.665400</td>
      <td>380136.537842</td>
      <td>0.0</td>
      <td>234916.791805</td>
      <td>621796.010176</td>
      <td>NaN</td>
      <td>7.451299</td>
      <td>8.024235</td>
      <td>0.572935</td>
    </tr>
    <tr>
      <th>0280\nProdução florestal; pesca e aquicultura</th>
      <td>0280</td>
      <td>20332</td>
      <td>2010</td>
      <td>3141.773938</td>
      <td>0.000000</td>
      <td>464.658777</td>
      <td>0.0</td>
      <td>-31766.265590</td>
      <td>-28159.832875</td>
      <td>NaN</td>
      <td>-1.385001</td>
      <td>-1.320025</td>
      <td>0.064976</td>
    </tr>
    <tr>
      <th>0580\nExtração de carvão mineral e de minerais não-metálicos</th>
      <td>0580</td>
      <td>14838</td>
      <td>2010</td>
      <td>755.047780</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>755.047780</td>
      <td>NaN</td>
      <td>0.050886</td>
      <td>0.181404</td>
      <td>0.130518</td>
    </tr>
    <tr>
      <th>0680\nExtração de petróleo e gás, inclusive as atividades de apoio</th>
      <td>0680</td>
      <td>117330</td>
      <td>2010</td>
      <td>1836.226776</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>1836.226776</td>
      <td>NaN</td>
      <td>0.01565</td>
      <td>0.678224</td>
      <td>0.662573</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8691 + 8692\nSaúde</th>
      <td>8691 + 8692</td>
      <td>217772</td>
      <td>2010</td>
      <td>176.221050</td>
      <td>110.838863</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>287.059913</td>
      <td>NaN</td>
      <td>0.001318</td>
      <td>0.008348</td>
      <td>0.00703</td>
    </tr>
    <tr>
      <th>9080\nAtividades artísticas, criativas e de espetáculos</th>
      <td>9080</td>
      <td>23299</td>
      <td>2010</td>
      <td>30.998257</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>30.998257</td>
      <td>NaN</td>
      <td>0.00133</td>
      <td>0.388011</td>
      <td>0.38668</td>
    </tr>
    <tr>
      <th>9480\nOrganizações associativas e outros serviços pessoais</th>
      <td>9480</td>
      <td>104279</td>
      <td>2010</td>
      <td>147.791306</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>147.791306</td>
      <td>NaN</td>
      <td>0.001417</td>
      <td>0.270885</td>
      <td>0.269468</td>
    </tr>
    <tr>
      <th>9700\nServiços domésticos</th>
      <td>9700</td>
      <td>40334</td>
      <td>2010</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.158532</td>
      <td>0.158532</td>
    </tr>
    <tr>
      <th>\nRESIDENCIAL</th>
      <td>RESIDENCIAL</td>
      <td>1618190</td>
      <td>2010</td>
      <td>24316.696992</td>
      <td>53450.119050</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>77766.816042</td>
      <td>NaN</td>
      <td>0.048058</td>
      <td>6.360248</td>
      <td>6.31219</td>
    </tr>
  </tbody>
</table>
<p>67 rows × 13 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-12ab0b96-7e75-4491-bd2d-683df9d6242f')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-12ab0b96-7e75-4491-bd2d-683df9d6242f button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-12ab0b96-7e75-4491-bd2d-683df9d6242f');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-167eed5c-0ba7-4e12-94ab-f6b223011faa">
  <button class="colab-df-quickchart" onclick="quickchart('df-167eed5c-0ba7-4e12-94ab-f6b223011faa')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-167eed5c-0ba7-4e12-94ab-f6b223011faa button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>





```python
#valores monetários deflacionados
sys = srn_defl.coef('2010', emission='total', household=True, ajust_sicor=True,reference_year='2011')
sys.result
```





  <div id="df-2672692f-70db-422f-8e65-b7c48b77d8d7" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>atividade_tru68_ibge</th>
      <th>production_values_mi_brl</th>
      <th>ano</th>
      <th>energia_Gg_CO2e_GWP_SAR</th>
      <th>residuo_Gg_CO2e_GWP_SAR</th>
      <th>agropecuaria_Gg_CO2e_GWP_SAR</th>
      <th>ippu_Gg_CO2e_GWP_SAR</th>
      <th>lulucf_Gg_CO2e_GWP_SAR</th>
      <th>total_Gg_CO2e_GWP_SAR</th>
      <th>active_loan_portfolio_mi_brl</th>
      <th>q_direct</th>
      <th>q_total</th>
      <th>q_indirect</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0191\nAgricultura, inclusive o apoio à agricultura e a pós-colheita</th>
      <td>0191</td>
      <td>183182.039144</td>
      <td>2010</td>
      <td>9835.322627</td>
      <td>0.000000</td>
      <td>72759.139369</td>
      <td>0.0</td>
      <td>49013.266995</td>
      <td>131607.728990</td>
      <td>NaN</td>
      <td>0.718453</td>
      <td>1.598446</td>
      <td>0.879993</td>
    </tr>
    <tr>
      <th>0192\nPecuária, inclusive o apoio à pecuária</th>
      <td>0192</td>
      <td>90525.194109</td>
      <td>2010</td>
      <td>5140.015130</td>
      <td>1602.665400</td>
      <td>380136.537842</td>
      <td>0.0</td>
      <td>234916.791805</td>
      <td>621796.010176</td>
      <td>NaN</td>
      <td>6.868762</td>
      <td>7.396906</td>
      <td>0.528144</td>
    </tr>
    <tr>
      <th>0280\nProdução florestal; pesca e aquicultura</th>
      <td>0280</td>
      <td>22056.349423</td>
      <td>2010</td>
      <td>3141.773938</td>
      <td>0.000000</td>
      <td>464.658777</td>
      <td>0.0</td>
      <td>-31766.265590</td>
      <td>-28159.832875</td>
      <td>NaN</td>
      <td>-1.276722</td>
      <td>-1.216826</td>
      <td>0.059896</td>
    </tr>
    <tr>
      <th>0580\nExtração de carvão mineral e de minerais não-metálicos</th>
      <td>0580</td>
      <td>16096.405309</td>
      <td>2010</td>
      <td>755.047780</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>755.047780</td>
      <td>NaN</td>
      <td>0.046908</td>
      <td>0.167222</td>
      <td>0.120314</td>
    </tr>
    <tr>
      <th>0680\nExtração de petróleo e gás, inclusive as atividades de apoio</th>
      <td>0680</td>
      <td>127280.714035</td>
      <td>2010</td>
      <td>1836.226776</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>1836.226776</td>
      <td>NaN</td>
      <td>0.014427</td>
      <td>0.6252</td>
      <td>0.610774</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8691 + 8692\nSaúde</th>
      <td>8691 + 8692</td>
      <td>236241.163018</td>
      <td>2010</td>
      <td>176.221050</td>
      <td>110.838863</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>287.059913</td>
      <td>NaN</td>
      <td>0.001215</td>
      <td>0.007695</td>
      <td>0.00648</td>
    </tr>
    <tr>
      <th>9080\nAtividades artísticas, criativas e de espetáculos</th>
      <td>9080</td>
      <td>25274.979599</td>
      <td>2010</td>
      <td>30.998257</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>30.998257</td>
      <td>NaN</td>
      <td>0.001226</td>
      <td>0.357676</td>
      <td>0.35645</td>
    </tr>
    <tr>
      <th>9480\nOrganizações associativas e outros serviços pessoais</th>
      <td>9480</td>
      <td>113122.863538</td>
      <td>2010</td>
      <td>147.791306</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>147.791306</td>
      <td>NaN</td>
      <td>0.001306</td>
      <td>0.249708</td>
      <td>0.248401</td>
    </tr>
    <tr>
      <th>9700\nServiços domésticos</th>
      <td>9700</td>
      <td>43754.711667</td>
      <td>2010</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.146138</td>
      <td>0.146138</td>
    </tr>
    <tr>
      <th>\nRESIDENCIAL</th>
      <td>RESIDENCIAL</td>
      <td>1755428.097204</td>
      <td>2010</td>
      <td>24316.696992</td>
      <td>53450.119050</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>77766.816042</td>
      <td>NaN</td>
      <td>0.044301</td>
      <td>5.863008</td>
      <td>5.818707</td>
    </tr>
  </tbody>
</table>
<p>67 rows × 13 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-2672692f-70db-422f-8e65-b7c48b77d8d7')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-2672692f-70db-422f-8e65-b7c48b77d8d7 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-2672692f-70db-422f-8e65-b7c48b77d8d7');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-9be7716f-c8f7-4132-8982-8596b64f033a">
  <button class="colab-df-quickchart" onclick="quickchart('df-9be7716f-c8f7-4132-8982-8596b64f033a')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-9be7716f-c8f7-4132-8982-8596b64f033a button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>




