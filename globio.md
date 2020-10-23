---
description: this is the spanish version
---

# GLOBIO

## Summary

The GLOBIO model provides an index of biodiversity foooooooooo according to mean species abundance \(MSA\), the average population-level response across a range of species, to different stressors, including land-use change, fragmentation, and infrastructure. The model can be used as a static assessment of how far below a pristine state the current environment is or to estimate how a change in any of the stressors would lead to a stress in biodiversity or ecosystem integrity, as indicated by MSA.

## Introduction

The GLOBIO methodology was developed by the United Nations Environmental Programme \(UNEP, Alkemade et al. 2009\) to model human impacts on biodiversity, measured by mean species abundance \(MSA\). Mean Species Abundance is an improvement over the more traditional species-area curve approach for two reasons. First, it gives aggregate estimates of species densities, not just species presence, which is important to represent true diversity since presence alone gives limited information about population viability \(Balmford et al 2012\). Second, it relates more than habitat area to changes in biodiversity by including information about the impact of fragmentation and threats from infrastructure \(and climate change and nitrogen deposition if the full version of GLOBIO were implemented\).

## The model

The GLOBIO method consists of a set of equations linking environmental drivers to biodiversity impact, tables of parameters to estimate the above equations, based on a broad literature review, and suggested methodologies for inputting and processing the spatial data required. We have extended the GLOBIO methodology to downscale their global approach to a landscape level.

### How it works

The GLOBIO approach is based on mean species abundance \(MSA, see Schwartz et al. 2003 for an example usage of MSA\). An MSA estimation ranges from 0 to 1, indicating the average proportional change in abundance of individual species in a location against that same location being pristine vegetation. An MSA of 1.0 implies that on average, species abundances are the same as in pristine land while an MSA of 0.0 implies that average species abundance is zero. Comparing MSA values of different scenarios identifies how anthropogenic changes affect biodiversity \(e.g., changing from a scenario with a mean MSA of 0.6 to a new scenario with 0.5 implies that, on average, individual species’ abundances declined 16.6% due to the land use change\). In GLOBIO, an MSA value is defined for every grid-cell within a geospatial extent.

Stressors decrease MSA in a multiplicative way. In the GLOBIO3 paper, the stressors included land use/land cover \(LU\), excess atmospheric nitrogen deposition \(N\), proximity of infrastructure \(mainly roads; I\), fragmentation \(F\) and climate change \(CC\), as in the following equation to calculate MSA per pixel \(i\):

To consider changes in land-use, we ignore the nitrogen deposition term and climate change terms; since neither of these terms change with each land-change scenario, they will cancel out when percent change in total \(summed\) MSA is calculated.  


We refine the GLOBIO methodology for MSA change due to infrastructure, fragmentation, and land-use in order to make use of higher resolution land-use/land-cover data \(500 m pixels from MODIS rather than 50 km pixels used by UNEP\) needed to detect finer-scale ecological response that may include nonlinearities. Downscaling requires new methods for assigning land management regime sub-classes with more precision based on high-resolution data rather than continent-wide aggregates, and a more sophisticated approach for quantifying fragmentation than applying overall averages of patch size for different habitats.

#### Calculating MSA Impact from Infrastructure

Table 1 provides the data from Alkemade et al. \(2009\) for MSA values of different buffers around infrastructure that impact different ecosystems. The impact of infrastructure on MSA is determined solely by distance, not by the nature of the intervening vegetation. An area of cropland that is 5 km from a road will have its MSA reduced by a factor of 0.9 regardless of whether the area between the cropland and the road is tropical forest or more cropland. All sources of infrastructure are aggregated into a “man-made” land use/land cover class. The remaining land cover classes, which can then be considered vegetative or “natural” are split into three basic types: tropical forest, temperate or boreal forest, and grassland or cropland. The distance of these different habitat classes from infrastructure is used to calculate the impact zone for determining MSA from infrastructure, using Table 1.

**Table 1: Effect of infrastructure impact zones on MSA, source:** Alkemade et al. \(2009\)

| **Impact Zone** | **Tropical Forest Distance to infrastructure \(m\)** | **Temperate Forest Distance to infrastructure \(m\)** | **Grassland & Cropland Distance to infrastructure \(m\)** | **MSA\_I** | **Standard Error** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| High Impact | &lt;1000 | &lt;300 | &lt;500 | 0.4 | 0.22 |
| Medium Impact | 1000-4000 | 300-1200 | 500-2000 | 0.8 | 0.13 |
| Low Impact | 4000-14,000 | 1200-4200 | 2000-7000 | 0.9 | 0.06 |
| No Impact | &gt;14,000 | &gt;4200 | &gt;7000 | 1.0 | 0.02 |

#### Calculating MSA Impact from Fragmentation

We augment the standard GLOBIO approach to fragmentation analysis by using a fragmented forest quality index \(FFQI\). The FFQI is similar to methods used in the forestry literature, and is calculated by considering how many of a forest’s neighboring cells are also forested. Rather than identifying the expected MSA impact from patch-size \(as in GLOBIO\), the FFQI estimates the relative effect of fragmentation with a Gaussian smoothing function. This treats habitat patches that are separated by only very small patches of infrastructure or non-habitat as less fragmented that habitat patches separated by wider distances. We convert the FFQI values on our map to km2 to match the zones defined in Table 2 \(according to Alkemade 2009\) by taking the square root of the area to convert it to the width/height of the patch. Although the method is different from how UNEP defined patches, comparisons to the literature showed FFQI to be an accurate approximation of more cumbersome patch-based approach.

**Table 2: Fragmentation effect on MSA under varying patch sizes, source:** Alkemade et al. \(2009\)

| **FFQI** | **Area \(km^2\)** | **MSA\_F** | **Standard Error** |
| :--- | :--- | :--- | :--- |
| &lt; 0.43 | &lt; 1 | 0.3 | 0.15 |
| 0.43 – 0.58 | &lt;10 | 0.6 | 0.19 |
| 0.58 – 0.90 | &lt;100 | 0.7 | 0.19 |
| 0.90 – 0.98 | &lt;1,000 | 0.9 | 0.20 |
| 0.98 – 0.99 | &lt;10,000 | 0.95 | 0.20 |
| 0.99 – 1 | &gt;10,000 | 1.0 | 0.20 |

#### Calculating MSA Impact from Land Use Change

The most difficult aspect of GLOBIO to implement is assigning different land-use/land-cover categories that relate to intensity of management or human use, since this information is often absent in remotely-sensed global land-cover datasets. To assist with this classification, we developed simple rules for reclassifying the MODIS or other satellite land-use/land-cover maps into the management categories for which MSA is quantified by GLOBIO’s broad literature reviews. Table 3 presents the rule-based categorization used to convert MODIS data to GLOBIO-compatible classes. LULC types that are mapped to more than one GLOBIO type are then split according to other auxiliary datasets described below.

**Forests:**

To distinguish between primary forest and other forest, including secondary \(replanted\) forests or forests with some extractive use and plantation forests, we analyze fragmentation in forest cover using FFQI and assign different use categories based on FFQI, with primary forest above a certain user-defined threshold. This approach assumes that pristine forests are more likely to be found in large, unfragmented tracts of forest, and that secondary or lightly used forests are more likely to be found in the most highly fragmented patches of forest. The threshold can be calibrated such that the aggregate amount of primary and secondary or lightly-used forests match estimates at the national or continental scale \(documented in Alkemade et al. 2009\).

**Shrubland and Grassland:**

To distinguish between primary vegetation \(more pristine\) grasslands, grazed grasslands, and man-made pastures \(deforested areas used for pasture\), we compare the potential vegetation map generated by Ramankutty and Foley \(1999\) described above to actual vegetation determined by MODIS land-cover data. If a particular pixel is designated forest according to the potential vegetation map, but is listed as grassland in MODIS, it has likely forest that has been cleared for grazing, in this case the pixel is reclassified as “man-made pasture.” If a pixel is grassland according to the potential vegetation map and is listed as grassland in the MODIS data, a separate dataset is utilized, quantifying the proportional pasture area at ~10 km resolution developed by Ramankutty et al. \(2008\). This pixel is defined as “livestock grazing” if the proportion of the grid-cell in pasture is greater than a user-defined threshold. The threshold can be chosen such that aggregate totals of livestock grazing match national and provincial data, as described above for forests. If the grassland pixel is lower than the grazing threshold, it will be defined as primary vegetation.

**Cropland:**

Because cropland intensification is only calculated in the MSALU and does not affect the configuration of primary habitat and thus the fragmentation calculated for MSAF, the spatial location of intensification is not necessary to define. The user only needs to designate the proportion of agriculture in the landscape that is intensified \(i.e., not low-input agriculture\). This can be found in the regional datasets cited by Alkemade et al. \(2009\) or available through FAO, or can be derived a dataset developed by Foley et al. \(2011\) that maps yield gaps for all major commodity crops globally at ~10 km resolution. This methodology compares agricultural production in similar climates \(based on precipitation and growing degree days\) and rates crop yield in different regions according to the maximum yields attained for its particular climate. The difference between actual and maximum attainable yield is defined as the “yield gap.” The yield gap can serve as a surrogate for \(lack of\) intensification, and the user can examine the yield gap maps for their region of interest to determine what proportion of the landscape falls below a certain level of yield gap.

**Table 3: MODIS to GLOBIO cover class conversion and MSA affected by land use**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>MODIS Land Use/Land Cover Class</b>
      </th>
      <th style="text-align:left"><b>Convert to which GLOBIO classes?</b>
      </th>
      <th style="text-align:left"><b>MSA_LU</b>
      </th>
      <th style="text-align:left"><b>SE</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">0 - Water</td>
      <td style="text-align:left">N/A</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>1 - Evergreen needleleaf forest</p>
        <p>2 - Evergreen broadleaf forest</p>
        <p>3 - Deciduous needleleaf forest</p>
        <p>4 - Deciduous broadleaf forest</p>
        <p>5 - Mixed forest</p>
      </td>
      <td style="text-align:left">
        <p>1 - Primary vegetationa</p>
        <p>3 - Secondary foresta</p>
      </td>
      <td style="text-align:left">
        <p>1</p>
        <p>0.5</p>
        <p>0.2</p>
      </td>
      <td style="text-align:left">
        <p>&lt;0.01</p>
        <p>0.03</p>
        <p>0.04</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>6 - Closed shrublands/cerrado</p>
        <p>7 - Open shrublands</p>
        <p>8 - Woody savannas</p>
        <p>9 - Savannas</p>
        <p>10 - Grasslands</p>
      </td>
      <td style="text-align:left">
        <p>1 - Primary vegetationb</p>
        <p>5 - Livestock grazingc</p>
        <p>6 - Man-made pasturesb</p>
      </td>
      <td style="text-align:left">
        <p>1</p>
        <p>0.7</p>
        <p>0.1</p>
      </td>
      <td style="text-align:left">
        <p>&lt;0.01</p>
        <p>0.05</p>
        <p>0.07</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">12 - Croplands/Perennial</td>
      <td style="text-align:left">12 &#x2013; All agriculture</td>
      <td style="text-align:left">
        <p>0.3</p>
        <p>0.1</p>
      </td>
      <td style="text-align:left">
        <p>0.12</p>
        <p>0.08</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">13 - Urban and built-up</td>
      <td style="text-align:left">10 - Built-up areas</td>
      <td style="text-align:left">0.05</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">16 - Barren or sparsely vegetated</td>
      <td style="text-align:left">1 - Primary vegetation</td>
      <td style="text-align:left">1.0</td>
      <td style="text-align:left">&lt;0.01</td>
    </tr>
  </tbody>
</table>

_Split based on \(a\)FFQ \(described in Fragmentation section, above\), \(b\) potential vegetation map \(Foley et al. 2009\), \(c\) proportional pasture area \(Ramunkutty et al. 2009\). Missing from this classification structure is GLOBIO classes “Lightly used natural forest” \(GLOBIO class 2\), “Plantation forest” \(GLOBIO class 4\), and “agroforestry” \(GLOBIO class 7\), “Low-input agriculture” \(GLOBIO class 8\), and “Intensive agriculture” \(GLOBIO class 9\). The agriculture classes are split in an aspatial calculation of MSA\_LU according to the “Fraction of intensification” value set by the user._

### Limitations and simplifications

MSA is an aggregate estimate, making it impossible to track compositional effects, and there are many different compositional possibilities for the same MSA. While MSA caps relative abundance of individual species at 1, ensuring that a local rise in one species cannot disguise a fall in overall species abundance, an MSA of 0.5 could mean that all species are half as abundant as in a pristine state, or that one species has suffered immense decline while the rest have remained constant, or anywhere in between. Additional information about the shape of the distribution of species abundances and extinction probabilities related to different levels of MSA could improve the usefulness of this index. But even then, diversity is more complex than numbers of species and population numbers. Some conservation biologists argue that species composition is as important as any other measure of diversity, and tracking specific species is essential to estimating impacts on threatened or endangered species or culturally valuable species \(Phalan et al. 2011 Food Policy\). To achieve this level of specificity, the impacts of different land-use strategies would need to be evaluated for each species individually and then combined across species for summary results, which may not be possible in many regions of the world with low data availability and high agricultural and other development pressure. In such cases, MSA provides a quick and easy to use index for biodiversity change in decision contexts.

In our application of GLOBIO, we use the mean parameter values and their standard errors to estimate the impacts of infrastructure, land-use, and fragmentation at new locations, which assumes that these values represent a random sample of species and geographic locations. However, limited data availability for certain taxonomic groups and geographic regions mean that there are potential biases in the parameter estimates that add an unquantifiable degree of uncertainty to predictions based on our application of GLOBIO.

The estimates of the impact of infrastructure are based on a meta-analysis of ~75 studies, predominately of bird and mammal populations in Europe and North America, with some information on insects and plants \(Alkemade et al. 2009; Benítez-López et al 2010\). Whether the impacts of infrastructure are similar for other taxonomic groups or geographic areas is unknown.

Estimates of the impacts of land use are based on a slightly greater number of studies, with 89 identified in the initial publication of GLOBIO \(Alkemade et al. 2009\) and 195 identified in a final published meta-analysis \(de Baan et al. 2013\). The parameter estimate for all artificial surfaces/built-up areas was based on expert opinion, representing densely populated cities, and without quantification of uncertainty \(Alkemade et al. 2009\). Datasets come largely from tropical regions, with fewer from temperate regions and none from boreal zones \(de Baan et al. 2013\). Data were available for 9 out of 14 biomes, and for many biomes, information was only available for some land use types. For example, information on permanent crops, agroforestry and artificial areas came only from two biomes. For three biomes, information was only available for pastures, but not for other land use types. As is common, data were also taxonomically biased towards vertebrate and plant species \(de Baan et al. 2013\). Arthropods were under-represented, and bacteria and fungi were not included at all in the database.

Furthermore, our assignment of satellite land-cover \(e.g., forest or grassland\) to the different GLOBIO land-use classes \(e.g., primary vs. secondary forest or pristine vs. grazed grassland\) introduce additional error that is not incorporated into the analysis. While we can ensure that our assignments aggregate up to national or regional level statistics, we cannot ground-truth our classification system to quantify the level of accuracy or uncertainty.

The impacts of fragmentation on mean species abundance \(MSA\) are based on six datasets from 3 publications. The proportion of species with a viable population was used as a proxy for MSA \(Alkemade et al. 2009\), and it is unclear how much additional uncertainty in the parameters that adds. Taxonomic and geographic biases are again a limitation. Two studies focus exclusively on mammals, including ~30 mammal species in Florida \(Allen et al. 2001\) and 10 species of carnivores from around the world \(Woodroffe & Ginsberg 1998\). The third study is limited exclusively to Europe, of which half of the 202 species included are birds \(Bouwma et al. 2002\).

## Data needs

The model uses 11 forms of input data. 3 are required and 8 are optional. **NOTE: All spatial data must be projected in meters \(i.e., a local, not a global or lat-long projection\), to ensure accurate distance to infrastructure calculations. The model will not execute without a defined projection.**

1. Land-use/cover map \(required\), following one of two options:
   1. Vegetation-specific \(not management-specific\) land-cover. This is the type of land-cover you may acquire from MODIS or other remotely-sensed data sources. It distinguishes between forest, grassland, savanna, cropland, and other vegetation types. It does NOT distinguish between the differences in management defined by GLOBIO, such as primary vs. secondary vegetation, or grassland vs. pasture. If this option is chosen, several helper datasets \(listed as required for option 1a, below\) will be required.
   2. Management-specific land-cover, following the classification scheme established by GLOBIO \(see Table 3, above\). If this option is chosen, tick the box for “Predefined land use map for GLOBIO” and enter the map there. All other data inputs will turn grey except for the other required data set, the infrastructure directory, and the optional AOI input.

Name: file can be named anything \(lulc\_2008.tif in the sample data\)

Format: standard GIS raster file \(e.g., ESRI GRID or IMG\), with a column labeled ‘value’ that designates the LULC class code for each cell \(integers only; e.g., 1 for forest, 10 for grassland, etc.\) The LULC ‘value’ codes must either match the LULC class codes used in the Land-cover to GLOBIO land-cover table described below \(if choosing option 1a\) or the GLOBIO land-cover specified in Table 3 \(if choosing 1b\). The table can have additional fields, but the only field used in this analysis is one for LULC class code.

1. Infrastructure directory \(required\). This is a folder containing maps of any forms of infrastructure you wish to consider in the calculation of MSAI. These data may be in either raster or vector format.

Name: folder can be named anything \(infrastructure\_dir in the sample data\)

Format: the files within the folder can be either raster or vector

1. Land-cover to GLOBIO land-cover table \(required for option 1a\). This is a table that translates the land-cover of option \(a\) in \(1\) above to intermediate GLOBIO classes, from which they will be further differentiated using the additional data below.

Name: file can be named anything \(lulc\_conversion\_table.csv in the sample data\)

File type: \*.csv

Rows: each row is a different LULC class.

Columns: the columns must be named as follows:

1. lucode: Land use and land cover class code of the dataset used. LULC codes match the ‘values’ column in the LULC raster of \(1a\) and must be numeric and unique.
2. globio\_lucode: The LULC code corresponding to the GLOBIO class to which it should be converted, using intermediate codes described in the example below.

_Example_: On the left is MODIS land-cover data, using the UMD classification, as defined in Table 3. On the right is the GLOBIO land-cover translation, which lumps the forest classes \(1-5 in MODIS\) into 130, grassland/shrubland \(6-10 in MODIS\) into 131, and agriculture \(12 in MODIS\) into 132. Urban land-use \(13 in MODIS\) maps directly onto built-up lands \(10 in GLOBIO\). Barren or sparsely vegetated \(16 in MODIS\) can be treated primary vegetation \(1 in GLOBIO\). The subsequent datasets and/or user inputs will help determine how to split up the 130, 131, and 132 into primary and secondary vegetation, rangelands and pasture, and intensified and unintensified agriculture, respectively.

| lucode | globio\_lucode |
| :--- | :--- |
| 0 | 0 |
| 1 | 130 |
| 2 | 130 |
| 3 | 130 |
| 4 | 130 |
| 5 | 130 |
| 6 | 131 |
| 7 | 131 |
| 8 | 131 |
| 9 | 131 |
| 10 | 131 |
| 12 | 132 |
| 13 | 10 |
| 16 | 1 |

1. Pasture map \(required for option 1a\). The proportional pasture area, as developed by Ramankutty et al. \(2008\). See explanation in _Shrubland and grassland_ under _How it Works_, above.

Name: file can be named anything \(pasture.tif in the sample data\)

Type: standard GIS raster file \(e.g., ESRI GRID or IMG\), with a column labeled ‘value’ that designates the proportion of the pixel that is in pasture \(restricted to floats between 0 and 1\).

1. Potential vegetation map \(required for option 1a\). This should be the potential vegetation map generated by Ramankutty and Foley \(1999\), or similar approach. It is important to use either this exact map or if using a different method for mapping potential vegetation, convert the land cover classifications to match those of this map. See explanation in _Shrubland and grassland_ under _How it Works_, above.

Name: file can be named anything \(potential\_vegetation.tif in the sample data\)

Type: standard GIS raster file \(e.g., ESRI GRID or IMG\), with a column labeled ‘value’ that designates the land cover class \(integers only\) according to Ramankutty and Foley \(1999\).

1. Primary Threshold \(required for option 1a\): a value between 0 and 1 that will determine the FFQI \(forest fragmentation quality index\) at which a cell should be assigned to primary or secondary forest, which can be adjusted such that the aggregate land-use matches regional statistics.
2. Pasture Threshold \(required for option 1a\): a value between 0 and 1 that will determine the proportion of pasture within a cell \(in the pasture map, input \#4\) in order for that cell to be assigned to grassland or livestock grazing, which can be adjusted such that the aggregate land-use matches regional statistics.
3. Proportion of Agriculture Intensified \(required for option 1a\): a value between 0 and 1 denoting the proportion of total agriculture that should be classified as “Intensive agriculture” or GLOBIO class 8 \(with 1 – Proportion of Agriculture Intensified being the proportion classified as “Low-input agriculture”, GLOBIO class 9\) in the computation of MSALU.
4. MSA parameter table \(required\). This table sets the values for MSA that should be used for the different impacts \(infrastructure, fragmentation and land-use\) to compute MSAI, MSAF, and MSALU. The example below \(and in the sample data\) gives the mean values and standard errors provided in Alkemade et al. \(2009\). This table can be altered to put high and low estimates from confidence intervals in the msa\_x column, to aid in uncertainty assessment.

Name: file can be named anything \(msa\_parameters.csv in sample data\)

Type: \*.csv

Columns: the columns must be named as follows:

* * 1. MSA\_type: either msa\_i\_primary, msa\_i\_other, msa\_f, or msa\_lu. The values for msa\_i are taken from Table 1 above, and msa\_i\_primary in the example below corresponds to the values used for tropical forest and msa\_i\_other corresponds to values used for grassland and cropland.
    2. Measurement: the metric by which the value in the subsequent column is measured.
    3. Value: the level of impact from which the MSA value is derived \(e.g., m of distance from infrastructure for msa\_i, the FFQI
    4. MSA\_x: the MSA set by Alkemade et al. \(2009\) for different types of impacts
    5. SE: the standard error associated with each MSA value, according to the meta-analysis in Alkemade et al. \(2009\). These values are not used by the model but are recorded here in this sample data set so that the user can adjust the MSA\_x values according to the confidence interval.

_Example_:

| MSA\_type | Measurement | Value | MSA\_x | SE |
| :--- | :--- | :--- | :--- | :--- |
| msa\_i\_primary | Distance \(m\) | &lt;1000 | 0.4 | 0.22 |
| msa\_i\_primary | Distance \(m\) | 1000-4000 | 0.8 | 0.13 |
| msa\_i\_primary | Distance \(m\) | 4000-14000 | 0.9 | 0.06 |
| msa\_i\_primary | Distance \(m\) | &gt;14000 | 1 | 0.02 |
| msa\_i\_other | Distance \(m\) | &lt;500 | 0.4 | 0.22 |
| msa\_i\_other | Distance \(m\) | 500-2000 | 0.8 | 0.13 |
| msa\_i\_other | Distance \(m\) | 2000-7000 | 0.9 | 0.06 |
| msa\_i\_other | Distance \(m\) | &gt;7000 | 1 | 0.02 |
| msa\_f | FFQI | &lt; 0.43 | 0.3 | 0.15 |
| msa\_f | FFQI | 0.43 - 0.58 | 0.6 | 0.19 |
| msa\_f | FFQI | 0.58 - 0.90 | 0.7 | 0.19 |
| msa\_f | FFQI | 0.90 - 0.98 | 0.9 | 0.2 |
| msa\_f | FFQI | 0.98 - 0.99 | 0.95 | 0.2 |
| msa\_f | FFQI | 0.99 - 1 | 1 | 0.2 |
| msa\_lu | Land Cover Class | 0 | 0 |  |
| msa\_lu | Land Cover Class | 1 | 1 | &lt;0.01 |
| msa\_lu | Land Cover Class | 2 | 0.7 | 0.07 |
| msa\_lu | Land Cover Class | 3 | 0.5 | 0.03 |
| msa\_lu | Land Cover Class | 4 | 0.2 | 0.04 |
| msa\_lu | Land Cover Class | 5 | 0.7 | 0.05 |
| msa\_lu | Land Cover Class | 6 | 0.1 | 0.07 |
| msa\_lu | Land Cover Class | 7 | 0.5 | 0.06 |
| msa\_lu | Land Cover Class | 8 | 0.3 | 0.12 |
| msa\_lu | Land Cover Class | 9 | 0.1 | 0.08 |
| msa\_lu | Land Cover Class | 10 | 0.05 | na |

1. AOI – Area of Interest \(optional\). If a summary of the MSA value is desired for the region, click the box next to AOI and enter a vector dataset containing the area\(s\) of interest, either as a region area or partitioned into subregions \(e.g., ecoregions, districts, etc.\).

Name: file can be named anything \(sub\_aoi.shp in the sample data\)

Type: polygon \(vector\) data

## Running the model

The model is available as a standalone application accessible from the install directory of InVEST \(under the subdirectory invest-3\_x86/invest\_globio.exe\).

### Advanced Usage

The GLOBIO model supports avoided re-computation. This means the model will detect intermediate and final results from a previous run in the specified workspace and it will avoid re-calculating any outputs that are identical to the previous run. This can save significant processing time for successive runs when only some input parameters have changed.

### Viewing Output from the Model

Upon successful completion of the model, a file explorer window will open to the output workspace specified in the model run. This directory contains an output folder holding files generated by this model. Those files can be viewed in any GIS tool such as ArcGIS, or QGIS. These files are described below in Section Interpreting Results.

## Interpreting Results

### Final Results

Final results are found within the _Workspace_ specified for this module.

* **globio-log**: Each time the model is run, a text \(.txt\) file will appear in the _Output_ folder. The file will list the parameter values for that run and will be named according to the service, the date and time, and the suffix.
* **aoi\_summary\_&lt;suffix&gt;**: A shapefile summarizing the average MSA for each zone defined in the area of interest.
* **msa\_&lt;suffix&gt;.tif**: A raster of the overall MSA \(mean species abundance\) value, defined as “the average abundances of originally occurring species relative to their abundance in the original, pristine or mature state as the basis.” This index is on a scale of 0 to 1, with 1 being the pristine condition, calculated as the product of the MSALU, MSAF, and MSAI below.
* **msa\_lu\_&lt;suffix&gt;.tif**: A raster of MSA calculated for impacts of land-use only.
* **msa\_f\_&lt;suffix&gt;.tif**: A raster of MSA calculated for impacts of fragmentation only.
* **msa\_i\_&lt;suffix&gt;.tif**: A raster of MSA calculated for impacts of infrastructure only.

### Intermediate Results

You may also want to examine the intermediate results. These files can help determine the reasons for the patterns in the final results. They are found in the _intermediate\_outputs_ folder within the _Workspace_ specified for this module.

* **distance\_to\_infrastructure\_&lt;suffix&gt;.tif**: A map coding each pixel by its distance to the nearest infrastructure, used to compute MSAI. Distance in this raster is measured as number of pixels, which is converted to meters in the model using the defined projection.
* **globio\_lulc\_&lt;suffix&gt;.tif**: The final land use map converted to GLOBIO classification, as outlined in Table 3. If desired, this map \(or any altered version of this map\) could be used to run the model using option 1b, above. This is used to compute MSALU.
* **primary\_veg\_smooth\_&lt;suffix&gt;.tif**: A Gaussian-filtered \(“smoothed”\) map of primary vegetation \(identified in globio\_lulc\), used to compute MSAF.
* **tmp/ffqi\_&lt;suffix&gt;.tif**: A map of the forest fragmentation quality index \(ffqi\), used to differentiate between primary and secondary forest in the GLOBIO land use classification.
* **tmp/combined\_infrastructure\_&lt;suffix&gt;.tif**: A map joining all the infrastructure files in the infrastructure directory \(input 2 above\). If there is only one file in that directory, it should be identical to that file.
* **tmp/:** Other files in this directory represent intermediate steps in calculations of the final data in the output folder.
* **\_taskgraph\_working\_dir:** This directory stores metadata used internally to enable avoided re-computation.

## References

Alkemade, Rob, Mark van Oorschot, Lera Miles, Christian Nellemann, Michel Bakkenes, and Ben ten Brink. "GLOBIO3: a framework to investigate options for reducing global terrestrial biodiversity loss." _Ecosystems_ 12, no. 3 \(2009\): 374-390.

Allen, C. R., Pearlstine, L. G., & Kitchens, W. M. \(2001\). Modeling viable mammal populations in gap analyses. Biological Conservation, 99\(2\), 135–144. doi:10.1016/S0006-3207\(00\)00084-7

Balmford A., R. Green, B. Phalan. 2012 What conservationists need to know about farming. Proc. R. Soc. B 279: 2714–2724.

Benítez-López, A., Alkemade, R., & Verweij, P. a. \(2010\). The impacts of roads and other infrastructure on mammal and bird populations: A meta-analysis. Biological Conservation, 143\(6\), 1307–1316. doi:10.1016/j.biocon.2010.02.009

Bouwma, I. M., Jongman, R. H. G., & Butovsky, R. O. \(2002\). Indicative map of the Pan-European Ecological Network - technical background document. Tilburg, The Netherlands/Budapest, Hungary.

de Baan, L., Alkemade, R., & Koellner, T. \(2013\). Land use impacts on biodiversity in LCA: a global approach. International Journal of Life Cycle Assessment, 18, 1216–1230. doi:10.1007/s11367-012-0412-0

Foley , J.A., et al. 2005. Global consequences of land use. Science 305: 570-574.

Foley, J.A., et al. 2011. Solutions for a cultivated planet. Nature 478: 337-342.

Mueller, N., et al. 2012. Closing yield gaps through nutrient and water management. Nature 490: 254-257.

Phalan, B., A. Balmford, R.E. Green, J.P.W. Scharlemann. 2011. Minimising the harm to biodiversity of producing more food globally. Food Policy 36: S62-S71.

Ramankutty, N. and J.A. Foley. 1999. Estimating Historical Changes in Global Land Cover: Croplands from 1700 to 1992, Global Biogeochemical Cycles, 13 \(4\), 997-1027

Ramankutty, N., et al. 2008. Farming the planet: 1. Geographic distribution of global agricultural lands in the year 2000. Global Biogeochemical Cycles, Vol. 22, GB1003

Woodroffe, R., & Ginsberg, J. R. \(1998\). Edge Effects and the Extinction of Populations Inside Protected Areas. Science, 280\(5372\), 2126–2128. doi:10.1126/science.280.5372.2126

