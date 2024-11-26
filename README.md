# Sirene Application

The Sirene application serves four main purposes:

1. **Facilitate Access to Raw Greenhouse Gas Emission Data**: Provides easy access to raw greenhouse gas (GHG) emission data generated by the MCTI (Ministry of Science, Technology, and Innovation).

2. **Access Disaggregated Emission Data**: Allows users to access disaggregated emission data using the disaggregation methodology (proposed in future IPEA paper).

3. **Estimate emission coefficients**: estimate total, direct, and indirect emission coefficients according to the methodology proposed by Luis Masa's paper.

4. **Continuous Improvement of Estimations**: Facilitates the review, critique, and suggestion of improvements to the methodology used for estimating emission coefficients. 

Additionally, the application provides sectoral data on Gross Production Value (VBP) and loan values by sector. With the disaggregated emission data and the VBP, we can estimate total, direct, and indirect emission coefficients according to the methodology proposed by Luis Masa's paper, titled ["AN ESTIMATION OF THE CARBON FOOTPRINT IN SPANISH CREDIT INSTITUTIONS’ BUSINESS LENDING PORTFOLIO"](https://repositorio.bde.es/bitstream/123456789/29610/4/do2220e.pdf).

Please note that the data are annual and, according to the latest SIRENE report, are available between the years 1990 and 2020.

---

## 1. Reading Raw Emission Data

The data are aggregated into five sectors: `'agropecuaria'`, `'energia'`, `'ippu'` (Industrial Processes and Product Use), `'lulucf'` (Land Use, Land-Use Change, and Forestry), and `'residuos'`, along with various subsectors. For each subsector, the MCTI estimates emissions of the main greenhouse gases: `'CO2'`, `'CH4'`, and `'N2O'`. Moreover, for a more comprehensive view, we can convert these emissions into CO₂ equivalents (CO₂e) using different methodologies: `'CO2e_GWP_SAR'`, `'CO2e_GWP_AR5'`, and `'CO2e_GTP_AR5'`.

### 1.1 Retrieving Emission Data of a Specific Sector


```python
#!pip install sirene
from sirene import srn_defl as srn
srn.read('agropecuaria')  # CO₂ emissions between 1990 and 2020 estimated by MCTI (6th edition)
srn.read('energia')
srn.read('ippu')
srn.read('lulucf')
srn.read('residuos').head(2) 
# srn.read('total-brazil-1', 'CO2e_GWP_SAR')
```




<div>
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
      <th>5. Resíduos</th>
      <td>533.281271</td>
      <td>543.542162</td>
      <td>564.801835</td>
      <td>535.723279</td>
      <td>563.93405</td>
      <td>584.509299</td>
      <td>681.307133</td>
      <td>732.83232</td>
      <td>777.821987</td>
      <td>906.700852</td>
      <td>...</td>
      <td>1114.443923</td>
      <td>1164.108223</td>
      <td>1229.771722</td>
      <td>1330.541005</td>
      <td>1148.902275</td>
      <td>503.660432</td>
      <td>654.8382</td>
      <td>514.6629</td>
      <td>374.4876</td>
      <td>234.3123</td>
    </tr>
    <tr>
      <th>5.A.   Disposição de Resíduos Sólidos</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 31 columns</p>
</div>



### 1.2 Emission Data for Different Gases


```python
srn.read('agropecuaria', 'CO2')          # Annual CO₂ emissions (in Gg) by the 'agriculture' sector
srn.read('agropecuaria', 'CH4')          # Annual CH₄ emissions
srn.read('agropecuaria', 'N2O').head(2)  # Annual N₂O emissions
```




<div>
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
      <th>3. Agropecuária</th>
      <td>279.66444</td>
      <td>285.562807</td>
      <td>294.191788</td>
      <td>297.69358</td>
      <td>307.340659</td>
      <td>312.686904</td>
      <td>297.197033</td>
      <td>307.411606</td>
      <td>315.603113</td>
      <td>319.361771</td>
      <td>...</td>
      <td>473.30052</td>
      <td>471.058836</td>
      <td>486.479602</td>
      <td>495.363993</td>
      <td>495.491183</td>
      <td>511.261527</td>
      <td>465.727887</td>
      <td>459.715568</td>
      <td>466.367715</td>
      <td>493.733508</td>
    </tr>
    <tr>
      <th>3.A.   Fermentação Entérica</th>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 31 columns</p>
</div>



### 1.3 Aggregating Emissions Using Different Methodologies


```python
srn.read('agropecuaria', 'CO2e_GWP_SAR')  # CO₂e emissions using the GWP_SAR methodology
srn.read('agropecuaria', 'CO2e_GWP_AR5')  # CO₂e emissions using the GWP_AR5 methodology
srn.read('agropecuaria', 'CO2e_GTP_AR5')  # CO₂e emissions using the GTP_AR5 methodology
srn.read('total-brasil-1', 'CO2e_GWP_SAR')
```




<div>
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



### 1.4 Identifying Subsectors of a Specific Sector


```python
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



### 1.5 Retrieving Data of a Specific Subsector


```python
srn.read('energia').loc['1.A.4.b.       Residencial', :].tail(5)
```




    4
    2016.0    18209.105762
    2017.0    18348.639738
    2018.0    18211.226425
    2019.0    18132.857115
    2020.0    18854.853915
    Name: 1.A.4.b.       Residencial, dtype: float64

---

## 2. Reading Disaggregated Emission Data

Each sector in the MCTI data (referred to as MCTI sectors) has been disaggregated into 68 activities based on the IBGE classification. This allows us to access the emissions of a specific activity within each sector (which could be considered the origin of emissions).

Note that not all years have been disaggregated. Disaggregation requires already disaggregated VBP data, which IBGE does not provide before 20XX.

For data that combine public and private health and education:


```python
srn.co2e_67.head(10)  # Sums of (8591 + 8592) and (8691 + 8692)
```




<div>
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

---

## 3. Scope of the Decomposition

Disaggregating MCTI emission data into 68 activities is a complex task. Due to this complexity, not all emissions have been assigned to a specific activity. Therefore, when comparing the total emissions of the 68 activities with the total estimated by the MCTI, the former is slightly lower. We have taken care to perform as much disaggregation as possible without significant loss of information to ensure data reliability.


```python
import warnings
import pandas as pd
import matplotlib.pyplot as plt
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

    /home/felipe/.local/lib/python3.10/site-packages/matplotlib/projections/__init__.py:63: UserWarning: Unable to import Axes3D. This may be due to multiple versions of Matplotlib being installed (e.g. as a system package and as a pip package). As a result, the 3D projection is not available.
      warnings.warn("Unable to import Axes3D. This may be due to multiple versions of "



    
![png](sirene_apresentacao_v3_files/sirene_apresentacao_v3_13_1.png)
    
---

## 4. Emission Coefficients

Using the disaggregated emission data and the deflated VBP (see relevant literature), we can estimate emission coefficients by activity (refer to the methodology proposed by Maza). To estimate these coefficients, we can consider the 'household' sector, which represents households responsible for emissions such as burning waste in rural areas.


### Data Field Descriptions

- **`atividade_tru68_ibge`**: Represents activity (i) classified under TRU68.
- **`production_values_mi_brl`**: Total production value of activity (i) for year (t), expressed in millions of BRL.
- **`ano`**: Reference year for the data (complete data available only between 2012 and 2020).
- **`energia_Gg_CO2e_GWP_SAR`**: CO2e emissions from the energy sector, measured in Gg and calculated using the CO2e_GWP_SAR methodology.
- **`residuo_Gg_CO2e_GWP_SAR`**: CO2e emissions from the waste sector, measured in Gg.
- **`agropecuaria_Gg_CO2e_GWP_SAR`**: CO2e emissions from the agricultural sector, measured in Gg.
- **`ippu_Gg_CO2e_GWP_SAR`**: CO2e emissions from the IPPU sector, measured in Gg.
- **`lulucf_Gg_CO2e_GWP_SAR`**: CO2e emissions from the LULUCF sector, measured in Gg.
- **`total_Gg_CO2e_GWP_SAR`**: Total CO2e emissions across all sectors, measured in Gg.
- **`active_loan_portfolio_mi_brl`**: Value of active loans by sector, expressed in millions of BRL.
- **`q_direct`**: Coefficient representing direct emissions.
- **`q_total`**: Coefficient representing total emissions (direct + indirect).
- **`q_indirect`**: Coefficient representing indirect emissions.



```python
coef_t = srn.coef(year='2020', emission='total', household=True, ajust_sicor=True, reference_year='2011')
coef_t.reference_year  # Reference year for deflation
# coef_t.adjust_sicor    # Adjust credit data
coef_t.defl_df         # DataFrame with deflation values
coef_t.defl_num        # Deflation scalar
coef_t.emission        # Type of emission: 'total', etc.
coef_t.household             # Boolean indicating inclusion of household sector
coef_t.mLeontiefBarr.shape   # Leontief matrix shape (67, 67)
coef_t.result[['atividade_tru68_ibge', 
               'production_values_mi_brl', 'ano']]   # IBGE data
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ano</th>
      <th>production_values_mi_brl</th>
      <th>total_Gg_CO2e_GWP_SAR</th>
      <th>ano</th>
      <th>q_direct</th>
      <th>q_total</th>
      <th>q_indirect</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0191\nAgricultura, inclusive o apoio à agricultura e a pós-colheita</th>
      <td>2020</td>
      <td>302059.919929</td>
      <td>223674.759909</td>
      <td>2020</td>
      <td>0.740498</td>
      <td>1.934712</td>
      <td>1.194214</td>
    </tr>
    <tr>
      <th>0192\nPecuária, inclusive o apoio à pecuária</th>
      <td>2020</td>
      <td>112305.027425</td>
      <td>942924.465575</td>
      <td>2020</td>
      <td>8.396102</td>
      <td>9.098249</td>
      <td>0.702147</td>
    </tr>
    <tr>
      <th>0280\nProdução florestal; pesca e aquicultura</th>
      <td>2020</td>
      <td>23393.557114</td>
      <td>-34593.308607</td>
      <td>2020</td>
      <td>-1.478754</td>
      <td>-1.409453</td>
      <td>0.069301</td>
    </tr>
    <tr>
      <th>0580\nExtração de carvão mineral e de minerais não-metálicos</th>
      <td>2020</td>
      <td>12780.776306</td>
      <td>419.651840</td>
      <td>2020</td>
      <td>0.032835</td>
      <td>0.157375</td>
      <td>0.12454</td>
    </tr>
    <tr>
      <th>0680\nExtração de petróleo e gás, inclusive as atividades de apoio</th>
      <td>2020</td>
      <td>131319.134611</td>
      <td>2611.296027</td>
      <td>2020</td>
      <td>0.019885</td>
      <td>0.515303</td>
      <td>0.495418</td>
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
    </tr>
    <tr>
      <th>8691 + 8692\nSaúde</th>
      <td>2020</td>
      <td>309474.34351</td>
      <td>173.138934</td>
      <td>2020</td>
      <td>0.000559</td>
      <td>0.004565</td>
      <td>0.004006</td>
    </tr>
    <tr>
      <th>9080\nAtividades artísticas, criativas e de espetáculos</th>
      <td>2020</td>
      <td>21619.992666</td>
      <td>30.165292</td>
      <td>2020</td>
      <td>0.001395</td>
      <td>0.487969</td>
      <td>0.486573</td>
    </tr>
    <tr>
      <th>9480\nOrganizações associativas e outros serviços pessoais</th>
      <td>2020</td>
      <td>94739.917992</td>
      <td>91.784941</td>
      <td>2020</td>
      <td>0.000969</td>
      <td>0.219711</td>
      <td>0.218742</td>
    </tr>
    <tr>
      <th>9700\nServiços domésticos</th>
      <td>2020</td>
      <td>32717.423064</td>
      <td>0.000000</td>
      <td>2020</td>
      <td>0.0</td>
      <td>0.104352</td>
      <td>0.104352</td>
    </tr>
    <tr>
      <th>\nRESIDENCIAL</th>
      <td>2020</td>
      <td>1756149.519048</td>
      <td>91105.313767</td>
      <td>2020</td>
      <td>0.051878</td>
      <td>5.601229</td>
      <td>5.549351</td>
    </tr>
  </tbody>
</table>
<p>67 rows × 7 columns</p>
</div>




```python
coef_t.result[['ano', 'production_values_mi_brl', 
               'total_Gg_CO2e_GWP_SAR', 'ano', 
               'q_direct', 'q_total', 'q_indirect']] # Emission coefficients as per Maza
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ano</th>
      <th>production_values_mi_brl</th>
      <th>total_Gg_CO2e_GWP_SAR</th>
      <th>ano</th>
      <th>q_direct</th>
      <th>q_total</th>
      <th>q_indirect</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0191\nAgricultura, inclusive o apoio à agricultura e a pós-colheita</th>
      <td>2020</td>
      <td>302059.919929</td>
      <td>223674.759909</td>
      <td>2020</td>
      <td>0.740498</td>
      <td>1.934712</td>
      <td>1.194214</td>
    </tr>
    <tr>
      <th>0192\nPecuária, inclusive o apoio à pecuária</th>
      <td>2020</td>
      <td>112305.027425</td>
      <td>942924.465575</td>
      <td>2020</td>
      <td>8.396102</td>
      <td>9.098249</td>
      <td>0.702147</td>
    </tr>
    <tr>
      <th>0280\nProdução florestal; pesca e aquicultura</th>
      <td>2020</td>
      <td>23393.557114</td>
      <td>-34593.308607</td>
      <td>2020</td>
      <td>-1.478754</td>
      <td>-1.409453</td>
      <td>0.069301</td>
    </tr>
    <tr>
      <th>0580\nExtração de carvão mineral e de minerais não-metálicos</th>
      <td>2020</td>
      <td>12780.776306</td>
      <td>419.651840</td>
      <td>2020</td>
      <td>0.032835</td>
      <td>0.157375</td>
      <td>0.12454</td>
    </tr>
    <tr>
      <th>0680\nExtração de petróleo e gás, inclusive as atividades de apoio</th>
      <td>2020</td>
      <td>131319.134611</td>
      <td>2611.296027</td>
      <td>2020</td>
      <td>0.019885</td>
      <td>0.515303</td>
      <td>0.495418</td>
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
    </tr>
    <tr>
      <th>8691 + 8692\nSaúde</th>
      <td>2020</td>
      <td>309474.34351</td>
      <td>173.138934</td>
      <td>2020</td>
      <td>0.000559</td>
      <td>0.004565</td>
      <td>0.004006</td>
    </tr>
    <tr>
      <th>9080\nAtividades artísticas, criativas e de espetáculos</th>
      <td>2020</td>
      <td>21619.992666</td>
      <td>30.165292</td>
      <td>2020</td>
      <td>0.001395</td>
      <td>0.487969</td>
      <td>0.486573</td>
    </tr>
    <tr>
      <th>9480\nOrganizações associativas e outros serviços pessoais</th>
      <td>2020</td>
      <td>94739.917992</td>
      <td>91.784941</td>
      <td>2020</td>
      <td>0.000969</td>
      <td>0.219711</td>
      <td>0.218742</td>
    </tr>
    <tr>
      <th>9700\nServiços domésticos</th>
      <td>2020</td>
      <td>32717.423064</td>
      <td>0.000000</td>
      <td>2020</td>
      <td>0.0</td>
      <td>0.104352</td>
      <td>0.104352</td>
    </tr>
    <tr>
      <th>\nRESIDENCIAL</th>
      <td>2020</td>
      <td>1756149.519048</td>
      <td>91105.313767</td>
      <td>2020</td>
      <td>0.051878</td>
      <td>5.601229</td>
      <td>5.549351</td>
    </tr>
  </tbody>
</table>
<p>67 rows × 7 columns</p>
</div>

---

## 5. Emissions from the Brazilian Credit Financial System

The application contains credit data by activity extracted from the Credit System maintained by the Central Bank of Brazil (BCB). Knowing the credit volume each sector receives and the emissions each sector produces, we can estimate the emissions per dollar lent by the financial system.

When set to `ajust_sicor=True`, it incorporates SICOR data when constructing emission coefficients. Otherwise, only SNC data is considered. SNC data already include SICOR data; however, in this case, they are classified as households (CPF). The more accurate approach is to treat this data as corporate entities (CNPJ), given the specific characteristics of the agricultural production system and its financing.

Credit System Data:


```python
loan = coef_t.result[['ano', 'production_values_mi_brl', 'total_Gg_CO2e_GWP_SAR', 'active_loan_portfolio_mi_brl']]  # Loan data
```




<div>
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
      <td>2020</td>
      <td>302059.919929</td>
      <td>223674.759909</td>
      <td>98827.307133</td>
    </tr>
    <tr>
      <th>0192\nPecuária, inclusive o apoio à pecuária</th>
      <td>2020</td>
      <td>112305.027425</td>
      <td>942924.465575</td>
      <td>61840.483836</td>
    </tr>
    <tr>
      <th>0280\nProdução florestal; pesca e aquicultura</th>
      <td>2020</td>
      <td>23393.557114</td>
      <td>-34593.308607</td>
      <td>2756.938589</td>
    </tr>
    <tr>
      <th>0580\nExtração de carvão mineral e de minerais não-metálicos</th>
      <td>2020</td>
      <td>12780.776306</td>
      <td>419.651840</td>
      <td>1040.023262</td>
    </tr>
    <tr>
      <th>0680\nExtração de petróleo e gás, inclusive as atividades de apoio</th>
      <td>2020</td>
      <td>131319.134611</td>
      <td>2611.296027</td>
      <td>470.467326</td>
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
      <td>2020</td>
      <td>309474.34351</td>
      <td>173.138934</td>
      <td>12731.747667</td>
    </tr>
    <tr>
      <th>9080\nAtividades artísticas, criativas e de espetáculos</th>
      <td>2020</td>
      <td>21619.992666</td>
      <td>30.165292</td>
      <td>2381.776043</td>
    </tr>
    <tr>
      <th>9480\nOrganizações associativas e outros serviços pessoais</th>
      <td>2020</td>
      <td>94739.917992</td>
      <td>91.784941</td>
      <td>5932.893967</td>
    </tr>
    <tr>
      <th>9700\nServiços domésticos</th>
      <td>2020</td>
      <td>32717.423064</td>
      <td>0.000000</td>
      <td>4.419046</td>
    </tr>
    <tr>
      <th>\nRESIDENCIAL</th>
      <td>2020</td>
      <td>1756149.519048</td>
      <td>91105.313767</td>
      <td>997684.753973</td>
    </tr>
  </tbody>
</table>
<p>67 rows × 4 columns</p>
</div>

