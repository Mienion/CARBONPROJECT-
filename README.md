# **Carbon Emission Analysis**
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.
Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

![Photo by Chris LeBoutillier (unsplash.com)](https://raw.githubusercontent.com/Mienion/CARBONPROJECT-/main/cover.jpg)

# **Data Source**
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

# **Data Structure**
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![structure](https://raw.githubusercontent.com/Mienion/CARBONPROJECT-/main/Database%20diagram.png)

# **Data Analysis Overview**

###### **1. Which products contribute the most to carbon emissions?**
```
SELECT product_name, carbon_footprint_pcf
FROM product_emissions
ORDER BY carbon_footprint_pcf DESC
LIMIT 10
```
| product_name                                                                                                                       | carbon_footprint_pcf | 
| ---------------------------------------------------------------------------------------------------------------------------------: | -------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044              | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187              | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608              | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625              | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687               | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000               | 
| TCDE                                                                                                                               | 99075                | 
| TCDE                                                                                                                               | 99075                | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000                | 
| Electric Motor                                                                                                                     | 87589                | 


###### **2. What are the industry groups of these products?**
```
with top_product_name AS 
(SELECT 
	industry_group_id,
	product_name, 
	carbon_footprint_pcf
FROM product_emissions
ORDER BY carbon_footprint_pcf DESC
LIMIT 10)

SELECT I.industry_group, SUM(T.carbon_footprint_pcf) AS "Total PCF"
FROM top_product_name T
LEFT JOIN industry_groups I ON T.industry_group_id = I.id
GROUP BY I.industry_group
ORDER BY "Total PCF" DESC
```
| industry_group                     | Total PCF | 
| ---------------------------------: | --------: | 
| Electrical Equipment and Machinery | 9778464   | 
| Automobiles & Components           | 282687    | 
| Materials                          | 365150    | 
| Capital Goods                      | 87589     | 


###### **3. What are the industries with the highest contribution to carbon emissions?**

```
SELECT 
	I.industry_group,
	SUM(P.carbon_footprint_pcf) AS "Total PCF"
FROM product_emissions P
LEFT JOIN industry_groups I ON P.industry_group_id = I.id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1
```

| industry_group                     | Total PCF | 
| ---------------------------------: | --------: | 
| Electrical Equipment and Machinery | 9801558   | 


###### **4. What are the companies with the highest contribution to carbon emissions?**
| company_name                           | Total PCF | 
| -------------------------------------: | --------: | 
| "Gamesa Corporación Tecnológica, S.A." | 9778464   | 


###### **5. What are the countries with the highest contribution to carbon emissions?**
```
SELECT 
	C.country_name,
	SUM(P.carbon_footprint_pcf) AS "Total PCF"
FROM product_emissions P
LEFT JOIN countries C ON P.country_id = C.id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1
```

| country_name | Total PCF | 
| -----------: | --------: | 
| Spain        | 9786130   | 

###### **6. What is the trend of carbon footprints (PCFs) over the years?**
```
SELECT 
	year,
	SUM(carbon_footprint_pcf) AS "Total PCF"
FROM product_emissions
GROUP BY year
ORDER BY year
```

| year | Total PCF | 
| ---: | --------: | 
| 2013 | 503857    | 
| 2014 | 624226    | 
| 2015 | 10840415  | 
| 2016 | 1640182   | 
| 2017 | 340271    | 

###### **7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?**
```
SELECT 
	year,
	industry_group,
	SUM(carbon_footprint_pcf) AS "Total PCF"
FROM product_emissions P
LEFT JOIN industry_groups I ON P.industry_group_id = I.id
GROUP BY 2, 1
ORDER BY 2, 1
```

| year | industry_group                                                         | Total PCF | 
| ---: | ---------------------------------------------------------------------: | --------: | 
| 2015 | "Consumer Durables, Household and Personal Products"                   | 931       | 
| 2013 | "Food, Beverage & Tobacco"                                             | 4995      | 
| 2014 | "Food, Beverage & Tobacco"                                             | 2685      | 
| 2015 | "Food, Beverage & Tobacco"                                             | 0         | 
| 2016 | "Food, Beverage & Tobacco"                                             | 100289    | 
| 2017 | "Food, Beverage & Tobacco"                                             | 3162      | 
| 2015 | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 8909      | 
| 2015 | "Mining - Iron, Aluminum, Other Metals"                                | 8181      | 
| 2013 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 32271     | 
| 2014 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 40215     | 
| 2015 | "Textiles, Apparel, Footwear and Luxury Goods"                         | 387       | 
| 2013 | Automobiles & Components                                               | 130189    | 
| 2014 | Automobiles & Components                                               | 230015    | 
| 2015 | Automobiles & Components                                               | 817227    | 
| 2016 | Automobiles & Components                                               | 1404833   | 
| 2013 | Capital Goods                                                          | 60190     | 
| 2014 | Capital Goods                                                          | 93699     | 
| 2015 | Capital Goods                                                          | 3505      | 
| 2016 | Capital Goods                                                          | 6369      | 
| 2017 | Capital Goods                                                          | 94949     | 
| 2015 | Chemicals                                                              | 62369     | 
| 2013 | Commercial & Professional Services                                     | 1157      | 
| 2014 | Commercial & Professional Services                                     | 477       | 
| 2016 | Commercial & Professional Services                                     | 2890      | 
| 2017 | Commercial & Professional Services                                     | 741       | 
| 2013 | Consumer Durables & Apparel                                            | 2867      | 
| 2014 | Consumer Durables & Apparel                                            | 3280      | 
| 2016 | Consumer Durables & Apparel                                            | 1162      | 
| 2015 | Containers & Packaging                                                 | 2988      | 
| 2015 | Electrical Equipment and Machinery                                     | 9801558   | 
| 2013 | Energy                                                                 | 750       | 
| 2016 | Energy                                                                 | 10024     | 
| 2015 | Food & Beverage Processing                                             | 141       | 
| 2014 | Food & Staples Retailing                                               | 773       | 
| 2015 | Food & Staples Retailing                                               | 706       | 
| 2016 | Food & Staples Retailing                                               | 2         | 
| 2015 | Gas Utilities                                                          | 122       | 
| 2013 | Household & Personal Products                                          | 0         | 
| 2013 | Materials                                                              | 200513    | 
| 2014 | Materials                                                              | 75678     | 
| 2016 | Materials                                                              | 88267     | 
| 2017 | Materials                                                              | 213137    | 
| 2013 | Media                                                                  | 9645      | 
| 2014 | Media                                                                  | 9645      | 
| 2015 | Media                                                                  | 1919      | 
| 2016 | Media                                                                  | 1808      | 
| 2014 | Retailing                                                              | 19        | 
| 2015 | Retailing                                                              | 11        | 
| 2014 | Semiconductors & Semiconductor Equipment                               | 50        | 
| 2016 | Semiconductors & Semiconductor Equipment                               | 4         | 
| 2015 | Semiconductors & Semiconductors Equipment                              | 3         | 
| 2013 | Software & Services                                                    | 6         | 
| 2014 | Software & Services                                                    | 146       | 
| 2015 | Software & Services                                                    | 22856     | 
| 2016 | Software & Services                                                    | 22846     | 
| 2017 | Software & Services                                                    | 690       | 
| 2013 | Technology Hardware & Equipment                                        | 61100     | 
| 2014 | Technology Hardware & Equipment                                        | 167361    | 
| 2015 | Technology Hardware & Equipment                                        | 106157    | 
| 2016 | Technology Hardware & Equipment                                        | 1566      | 
| 2017 | Technology Hardware & Equipment                                        | 27592     | 
| 2013 | Telecommunication Services                                             | 52        | 
| 2014 | Telecommunication Services                                             | 183       | 
| 2015 | Telecommunication Services                                             | 183       | 
| 2015 | Tires                                                                  | 2022      | 
| 2015 | Tobacco                                                                | 1         | 
| 2015 | Trading Companies & Distributors and Commercial Services & Supplies    | 239       | 
| 2013 | Utilities                                                              | 122       | 
| 2016 | Utilities                                                              | 122       | 

**Analysis Summary**

*Products contributing the most to carbon emissions are Wind Turbine, automotive industry products (Land Cruiser, Mercedes-Benz). Industry groups of these products are Electrical Equipment and Machinery,  Automobiles & Components, Materials and Capital Goods. The industries with the highest contribution to carbon emissions is  Electrical Equipment and Machinery. The companies with the highest contribution to carbon emissions is Gamesa Corporación Tecnológica, S.A. The countries with the highest contribution to carbon emissions is Spain.*

*The carbon emissions from industrial sectors showed an increasing trend during the period from 2013 to 2015, with a particularly sharp surge in 2015 — reaching a level 21 times higher than in 2013. However, from 2015 to 2017, emissions decreased significantly. By 2017, the carbon emission volume had dropped to 347,271 PCF, which represents a 31-fold decrease compared to 2015 and a 48% reduction compared to 2013. The industry groups have demonstrated the notable decrease in carbon footprints (PCFs) over time is Media, Food & Staples Retailing.*


