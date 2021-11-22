# Housing Rental Analysis for San Francisco

In this challenge, your job is to use your data visualization skills, including aggregation, interactive visualizations, and geospatial analysis, to find properties in the San Francisco market that are viable investment opportunities.

## Instructions

Use the `san_francisco_housing.ipynb` notebook to visualize and analyze the real-estate data.

Note that this assignment requires you to create a visualization by using hvPlot and GeoViews. Additionally, you need to read the `sfo_neighborhoods_census_data.csv` file from the `Resources` folder into the notebook and create the DataFrame that you’ll use in the analysis.

The main task in this Challenge is to visualize and analyze the real-estate data in your Jupyter notebook. Use the `san_francisco_housing.ipynb` notebook to complete the following tasks:

* Calculate and plot the housing units per year.

* Calculate and plot the average prices per square foot.

* Compare the average prices by neighborhood.

* Build an interactive neighborhood map.

* Compose your data story.

### Calculate and Plot the Housing Units per Year

For this part of the assignment, use numerical and visual aggregation to calculate the number of housing units per year, and then visualize the results as a bar chart. To do so, complete the following steps:

1. Use the `groupby` function to group the data by year. Aggregate the results by the `mean` of the groups.

2. Use the `hvplot` function to plot the `housing_units_by_year` DataFrame as a bar chart. Make the x-axis represent the `year` and the y-axis represent the `housing_units`.

3. Style and format the line plot to ensure a professionally styled visualization.

4. Note that your resulting plot should appear similar to the following image:

![A screenshot depicts an example of the resulting bar chart.](Images/zoomed-housing-units-by-year.png)

5. Answer the following question:

    * What’s the overall trend in housing units over the period that you’re analyzing?

### Calculate and Plot the Average Sale Prices per Square Foot

For this part of the assignment, use numerical and visual aggregation to calculate the average prices per square foot, and then visualize the results as a bar chart. To do so, complete the following steps:

1. Group the data by year, and then average the results. What’s the lowest gross rent that’s reported for the years that the DataFrame includes?

2. Create a new DataFrame named `prices_square_foot_by_year` by filtering out the “housing_units” column. The new DataFrame should include the averages per year for only the sale price per square foot and the gross rent.

3. Use hvPlot to plot the `prices_square_foot_by_year` DataFrame as a line plot.

    > **Hint** This single plot will include lines for both `sale_price_sqr_foot` and `gross_rent`.

4. Style and format the line plot to ensure a professionally styled visualization.

5. Note that your resulting plot should appear similar to the following image:

![A screenshot depicts an example of the resulting plot.](Images/avg-sale-px-sq-foot-gross-rent.png)

6. Use both the `prices_square_foot_by_year` DataFrame and interactive plots to answer the following questions:

    * Did any year experience a drop in the average sale price per square foot compared to the previous year?

    * If so, did the gross rent increase or decrease during that year?

### Compare the Average Sale Prices by Neighborhood

For this part of the assignment, use interactive visualizations and widgets to explore the average sale price per square foot by neighborhood. To do so, complete the following steps:

1. Create a new DataFrame that groups the original DataFrame by year and neighborhood. Aggregate the results by the `mean` of the groups.

2. Filter out the “housing_units” column to create a DataFrame that includes only the `sale_price_sqr_foot` and `gross_rent` averages per year.

3. Create an interactive line plot with hvPlot that visualizes both `sale_price_sqr_foot` and `gross_rent`. Set the x-axis parameter to the year (`x="year"`). Use the `groupby` parameter to create an interactive widget for `neighborhood`.

4. Style and format the line plot to ensure a professionally styled visualization.

5. Note that your resulting plot should appear similar to the following image:

![A screenshot depicts an example of the resulting plot.](Images/pricing-info-by-neighborhood.png)

6. Use the interactive visualization to answer the following question:

    * For the Anza Vista neighborhood, is the average sale price per square foot for 2016 more or less than the price that’s listed for 2012? 

### Build an Interactive Neighborhood Map

For this part of the assignment, explore the geospatial relationships in the data by using interactive visualizations with hvPlot and GeoViews. To build your map, use the `sfo_data_df` DataFrame (created during the initial import), which includes the neighborhood location data with the average prices. To do all this, complete the following steps:

1. Read the `neighborhood_coordinates.csv` file from the `Resources` folder into the notebook, and create a DataFrame named `neighborhood_locations_df`. Be sure to set the `index_col` of the DataFrame as “Neighborhood”.

2. Using the original `sfo_data_df` Dataframe, create a DataFrame named `all_neighborhood_info_df` that groups the data by neighborhood. Aggregate the results by the `mean` of the group.

3. Review the two code cells that concatenate the `neighborhood_locations_df` DataFrame with the `all_neighborhood_info_df` DataFrame. Note that the first cell uses the [Pandas concat function](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html) to create a DataFrame named `all_neighborhoods_df`. The second cell cleans the data and sets the “Neighborhood” column. Be sure to run these cells to create the `all_neighborhoods_df` DataFrame, which you’ll need to create the geospatial visualization.

4. Using hvPlot with GeoViews enabled, create a `points` plot for the `all_neighborhoods_df` DataFrame. Be sure to do the following:

    * Set the `geo` parameter to True.
    * Set the `size` parameter to “sale_price_sqr_foot”.
    * Set the `color` parameter to “gross_rent”.
    * Set the `frame_width` parameter to 700.
    * Set the `frame_height` parameter to 500.
    * Include a descriptive title.

Note that your resulting plot should appear similar to the following image:

![A screenshot depicts an example of a scatter plot created with hvPlot and GeoViews.](Images/6-4-geoviews-plot.png)

5. Use the interactive map to answer the following question:

    * Which neighborhood has the highest gross rent, and which has the highest sale price per square foot?

### Compose Your Data Story

Based on the visualizations that you created, answer the following questions:

* How does the trend in rental income growth compare to the trend in sales prices? Does this same trend hold true for all the neighborhoods across San Francisco?

* What insights can you share with your company about the potential one-click, buy-and-rent strategy that they're pursuing? Do neighborhoods exist that you would suggest for investment, and why?


```python
# Import the required libraries and dependencies
import pandas as pd
import hvplot.pandas
from pathlib import Path
%matplotlib inline
```

## Import the data 


```python
# Using the read_csv function and Path module, create a DataFrame 
# by importing the sfo_neighborhoods_census_data.csv file from the Resources folder
sfo_data_df = pd.read_csv(Path("../Starter_Code/sfo_neighborhoods_census_data.csv"), index_col="year")
#sfo_data_df = pd.read_csv(file_path, index_col="year")
# Review the first and last five rows of the DataFrame
sfo_data_df.head()
sfo_data_df.tail()
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
      <th>neighborhood</th>
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>Telegraph Hill</td>
      <td>903.049771</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>Twin Peaks</td>
      <td>970.085470</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>Van Ness/ Civic Center</td>
      <td>552.602567</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>Visitacion Valley</td>
      <td>328.319007</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>Westwood Park</td>
      <td>631.195426</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
  </tbody>
</table>
</div>



---

## Calculate and Plot the Housing Units per Year

For this part of the assignment, use numerical and visual aggregation to calculate the number of housing units per year, and then visualize the results as a bar chart. To do so, complete the following steps:

1. Use the `groupby` function to group the data by year. Aggregate the results by the `mean` of the groups.

2. Use the `hvplot` function to plot the `housing_units_by_year` DataFrame as a bar chart. Make the x-axis represent the `year` and the y-axis represent the `housing_units`.

3. Style and format the line plot to ensure a professionally styled visualization.

4. Note that your resulting plot should appear similar to the following image:

![A screenshot depicts an example of the resulting bar chart.](Images/zoomed-housing-units-by-year.png)

5. Answer the following question:

    * What’s the overall trend in housing units over the period that you’re analyzing?



### Step 1: Use the `groupby` function to group the data by year. Aggregate the results by the `mean` of the groups.


```python
# Create a numerical aggregation that groups the data by the year and then averages the results.
housing_units_by_year = sfo_data_df.groupby('year').mean()
# Review the DataFrame
housing_units_by_year
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
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2010</th>
      <td>369.344353</td>
      <td>372560</td>
      <td>1239</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>341.903429</td>
      <td>374507</td>
      <td>1530</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>399.389968</td>
      <td>376454</td>
      <td>2324</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>483.600304</td>
      <td>378401</td>
      <td>2971</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>556.277273</td>
      <td>380348</td>
      <td>3528</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>632.540352</td>
      <td>382295</td>
      <td>3739</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>697.643709</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
  </tbody>
</table>
</div>



### Step 2: Use the `hvplot` function to plot the `housing_units_by_year` DataFrame as a bar chart. Make the x-axis represent the `year` and the y-axis represent the `housing_units`.

### Step 3: Style and format the line plot to ensure a professionally styled visualization.


```python
# Create a visual aggregation explore the housing units by year
#how to rescale the y axis
housing_units_by_year.hvplot.bar(
    x="year",
    y="housing_units",
    title="Housing Units in San Fancisco from 2010 to 2016",
    color="blue",
    xlabel="Year",
    ylabel="Housing Units"
).opts(yformatter='%.0f')
```






<div id='2060'>





  <div class="bk-root" id="5074531c-0052-40fa-b1d9-48f3b7ab4655" data-root-id="2060"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"1ff284aa-92cb-4d28-ae21-b88e1916e268":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"bottom":{"value":0},"fill_alpha":{"value":1.0},"fill_color":{"value":"blue"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"blue"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"top":{"field":"housing_units"},"width":{"value":0.8},"x":{"field":"year"}},"id":"2102","type":"VBar"},{"attributes":{},"id":"2078","type":"BasicTicker"},{"attributes":{"axis":{"id":"2074"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2076","type":"Grid"},{"attributes":{},"id":"2083","type":"WheelZoomTool"},{"attributes":{"overlay":{"id":"2086"}},"id":"2084","type":"BoxZoomTool"},{"attributes":{},"id":"2082","type":"PanTool"},{"attributes":{},"id":"2106","type":"AllLabels"},{"attributes":{},"id":"2075","type":"CategoricalTicker"},{"attributes":{"axis_label":"Year","coordinates":null,"formatter":{"id":"2105"},"group":null,"major_label_policy":{"id":"2106"},"ticker":{"id":"2075"}},"id":"2074","type":"CategoricalAxis"},{"attributes":{"coordinates":null,"group":null,"text":"Housing Units in San Fancisco from 2010 to 2016","text_color":"black","text_font_size":"12pt"},"id":"2066","type":"Title"},{"attributes":{"children":[{"id":"2061"},{"id":"2065"},{"id":"2130"}],"margin":[0,0,0,0],"name":"Row05077","tags":["embedded"]},"id":"2060","type":"Row"},{"attributes":{},"id":"2085","type":"ResetTool"},{"attributes":{"format":"%.0f"},"id":"2103","type":"PrintfTickFormatter"},{"attributes":{"factors":["2010","2011","2012","2013","2014","2015","2016"],"tags":[[["year","year",null]]]},"id":"2062","type":"FactorRange"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"blue"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"blue"},"line_alpha":{"value":0.1},"top":{"field":"housing_units"},"width":{"value":0.8},"x":{"field":"year"}},"id":"2098","type":"VBar"},{"attributes":{"below":[{"id":"2074"}],"center":[{"id":"2076"},{"id":"2080"}],"height":300,"left":[{"id":"2077"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"2100"}],"sizing_mode":"fixed","title":{"id":"2066"},"toolbar":{"id":"2087"},"width":700,"x_range":{"id":"2062"},"x_scale":{"id":"2070"},"y_range":{"id":"2063"},"y_scale":{"id":"2072"}},"id":"2065","subtype":"Figure","type":"Plot"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer05082","sizing_mode":"stretch_width"},"id":"2130","type":"Spacer"},{"attributes":{"axis":{"id":"2077"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2080","type":"Grid"},{"attributes":{"data":{"housing_units":[372560,374507,376454,378401,380348,382295,384242],"year":["2010","2011","2012","2013","2014","2015","2016"]},"selected":{"id":"2095"},"selection_policy":{"id":"2118"}},"id":"2094","type":"ColumnDataSource"},{"attributes":{"axis_label":"Housing Units","coordinates":null,"formatter":{"id":"2103"},"group":null,"major_label_policy":{"id":"2109"},"ticker":{"id":"2078"}},"id":"2077","type":"LinearAxis"},{"attributes":{"source":{"id":"2094"}},"id":"2101","type":"CDSView"},{"attributes":{"fill_color":{"value":"blue"},"hatch_color":{"value":"blue"},"top":{"field":"housing_units"},"width":{"value":0.8},"x":{"field":"year"}},"id":"2097","type":"VBar"},{"attributes":{},"id":"2109","type":"AllLabels"},{"attributes":{},"id":"2072","type":"LinearScale"},{"attributes":{},"id":"2095","type":"Selection"},{"attributes":{"callback":null,"renderers":[{"id":"2100"}],"tags":["hv_created"],"tooltips":[["year","@{year}"],["housing_units","@{housing_units}"]]},"id":"2064","type":"HoverTool"},{"attributes":{"tools":[{"id":"2064"},{"id":"2081"},{"id":"2082"},{"id":"2083"},{"id":"2084"},{"id":"2085"}]},"id":"2087","type":"Toolbar"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2086","type":"BoxAnnotation"},{"attributes":{},"id":"2081","type":"SaveTool"},{"attributes":{},"id":"2118","type":"UnionRenderers"},{"attributes":{},"id":"2070","type":"CategoricalScale"},{"attributes":{"coordinates":null,"data_source":{"id":"2094"},"glyph":{"id":"2097"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2099"},"nonselection_glyph":{"id":"2098"},"selection_glyph":{"id":"2102"},"view":{"id":"2101"}},"id":"2100","type":"GlyphRenderer"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"blue"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"blue"},"line_alpha":{"value":0.2},"top":{"field":"housing_units"},"width":{"value":0.8},"x":{"field":"year"}},"id":"2099","type":"VBar"},{"attributes":{},"id":"2105","type":"CategoricalTickFormatter"},{"attributes":{"end":385410.2,"reset_end":385410.2,"reset_start":0.0,"tags":[[["housing_units","housing_units",null]]]},"id":"2063","type":"Range1d"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer05081","sizing_mode":"stretch_width"},"id":"2061","type":"Spacer"}],"root_ids":["2060"]},"title":"Bokeh Application","version":"2.4.1"}};
    var render_items = [{"docid":"1ff284aa-92cb-4d28-ae21-b88e1916e268","root_ids":["2060"],"roots":{"2060":"5074531c-0052-40fa-b1d9-48f3b7ab4655"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



### Step 5: Answer the following question:

**Question:** What is the overall trend in housing_units over the period being analyzed?

**Answer:** There's a general uptrend in the housing units sold over the years.  It's not an insane leap from year to year, but more like little by little it increases each year.

---

## Calculate and Plot the Average Sale Prices per Square Foot

For this part of the assignment, use numerical and visual aggregation to calculate the average prices per square foot, and then visualize the results as a bar chart. To do so, complete the following steps:

1. Group the data by year, and then average the results. What’s the lowest gross rent that’s reported for the years that the DataFrame includes?

2. Create a new DataFrame named `prices_square_foot_by_year` by filtering out the “housing_units” column. The new DataFrame should include the averages per year for only the sale price per square foot and the gross rent.

3. Use hvPlot to plot the `prices_square_foot_by_year` DataFrame as a line plot.

    > **Hint** This single plot will include lines for both `sale_price_sqr_foot` and `gross_rent`.

4. Style and format the line plot to ensure a professionally styled visualization.

5. Note that your resulting plot should appear similar to the following image:

![A screenshot depicts an example of the resulting plot.](Images/avg-sale-px-sq-foot-gross-rent.png)

6. Use both the `prices_square_foot_by_year` DataFrame and interactive plots to answer the following questions:

    * Did any year experience a drop in the average sale price per square foot compared to the previous year?

    * If so, did the gross rent increase or decrease during that year?



### Step 1: Group the data by year, and then average the results.


```python
# Create a numerical aggregation by grouping the data by year and averaging the results
prices_square_foot_by_year = sfo_data_df.groupby(['year']).mean()

# Review the resulting DataFrame
prices_square_foot_by_year

#isn't this already done in a cell above and we're just renaming it?
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
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2010</th>
      <td>369.344353</td>
      <td>372560</td>
      <td>1239</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>341.903429</td>
      <td>374507</td>
      <td>1530</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>399.389968</td>
      <td>376454</td>
      <td>2324</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>483.600304</td>
      <td>378401</td>
      <td>2971</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>556.277273</td>
      <td>380348</td>
      <td>3528</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>632.540352</td>
      <td>382295</td>
      <td>3739</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>697.643709</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
  </tbody>
</table>
</div>



**Question:** What is the lowest gross rent reported for the years included in the DataFrame?

**Answer:** Rent in 2010.

### Step 2: Create a new DataFrame named `prices_square_foot_by_year` by filtering out the “housing_units” column. The new DataFrame should include the averages per year for only the sale price per square foot and the gross rent.


```python
# Filter out the housing_units column, creating a new DataFrame 
# Keep only sale_price_sqr_foot and gross_rent averages per year
prices_square_foot_by_year = sfo_data_df.drop(columns=['housing_units']).groupby(['year']).mean()

# Review the DataFrame

prices_square_foot_by_year
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
      <th>sale_price_sqr_foot</th>
      <th>gross_rent</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2010</th>
      <td>369.344353</td>
      <td>1239</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>341.903429</td>
      <td>1530</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>399.389968</td>
      <td>2324</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>483.600304</td>
      <td>2971</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>556.277273</td>
      <td>3528</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>632.540352</td>
      <td>3739</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>697.643709</td>
      <td>4390</td>
    </tr>
  </tbody>
</table>
</div>



### Step 3: Use hvPlot to plot the `prices_square_foot_by_year` DataFrame as a line plot.

> **Hint** This single plot will include lines for both `sale_price_sqr_foot` and `gross_rent`

### Step 4: Style and format the line plot to ensure a professionally styled visualization.



```python
# Plot prices_square_foot_by_year. 
# Inclued labels for the x- and y-axes, and a title.
prices_square_foot_by_year.hvplot(title="Sale Price per Square Foot and Average Gross Rent for 2010 - 2016 - San Fran.", ylabel='Gross Rent/Sale Price per Sqr Foot', xlabel='year', figsize=(20,10))
```






<div id='2177'>





  <div class="bk-root" id="a7589aa4-39bc-4977-9b01-6888458d81ec" data-root-id="2177"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"53180ae3-849c-4965-8723-09a2652e8e28":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"2240","type":"UnionRenderers"},{"attributes":{},"id":"2263","type":"UnionRenderers"},{"attributes":{"line_color":"#fc4f30","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2249","type":"Line"},{"attributes":{},"id":"2206","type":"WheelZoomTool"},{"attributes":{"end":2016.0,"reset_end":2016.0,"reset_start":2010.0,"start":2010.0,"tags":[[["year","year",null]]]},"id":"2181","type":"Range1d"},{"attributes":{"label":{"value":"gross_rent"},"renderers":[{"id":"2252"}]},"id":"2266","type":"LegendItem"},{"attributes":{"axis_label":"Gross Rent/Sale Price per Sqr Foot","coordinates":null,"formatter":{"id":"2223"},"group":null,"major_label_policy":{"id":"2224"},"ticker":{"id":"2201"}},"id":"2200","type":"LinearAxis"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2244"},{"id":"2266"}],"location":[0,0],"title":"Variable"},"id":"2243","type":"Legend"},{"attributes":{"source":{"id":"2225"}},"id":"2232","type":"CDSView"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2209","type":"BoxAnnotation"},{"attributes":{"children":[{"id":"2178"},{"id":"2187"},{"id":"2376"}],"margin":[0,0,0,0],"name":"Row05249","tags":["embedded"]},"id":"2177","type":"Row"},{"attributes":{"source":{"id":"2246"}},"id":"2253","type":"CDSView"},{"attributes":{"coordinates":null,"group":null,"text":"Sale Price per Square Foot and Average Gross Rent for 2010 - 2016 - San Fran.","text_color":"black","text_font_size":"12pt"},"id":"2188","type":"Title"},{"attributes":{},"id":"2192","type":"LinearScale"},{"attributes":{},"id":"2224","type":"AllLabels"},{"attributes":{},"id":"2208","type":"ResetTool"},{"attributes":{"tools":[{"id":"2183"},{"id":"2204"},{"id":"2205"},{"id":"2206"},{"id":"2207"},{"id":"2208"}]},"id":"2210","type":"Toolbar"},{"attributes":{"axis":{"id":"2196"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2199","type":"Grid"},{"attributes":{},"id":"2223","type":"BasicTickFormatter"},{"attributes":{"line_color":"#fc4f30","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2267","type":"Line"},{"attributes":{"axis":{"id":"2200"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2203","type":"Grid"},{"attributes":{"coordinates":null,"data_source":{"id":"2246"},"glyph":{"id":"2249"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2251"},"nonselection_glyph":{"id":"2250"},"selection_glyph":{"id":"2267"},"view":{"id":"2253"}},"id":"2252","type":"GlyphRenderer"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2229","type":"Line"},{"attributes":{"data":{"Variable":["gross_rent","gross_rent","gross_rent","gross_rent","gross_rent","gross_rent","gross_rent"],"value":[1239,1530,2324,2971,3528,3739,4390],"year":[2010,2011,2012,2013,2014,2015,2016]},"selected":{"id":"2247"},"selection_policy":{"id":"2263"}},"id":"2246","type":"ColumnDataSource"},{"attributes":{},"id":"2205","type":"PanTool"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2250","type":"Line"},{"attributes":{"overlay":{"id":"2209"}},"id":"2207","type":"BoxZoomTool"},{"attributes":{},"id":"2221","type":"AllLabels"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer05254","sizing_mode":"stretch_width"},"id":"2376","type":"Spacer"},{"attributes":{},"id":"2204","type":"SaveTool"},{"attributes":{"line_color":"#30a2da","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2228","type":"Line"},{"attributes":{"line_color":"#30a2da","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2245","type":"Line"},{"attributes":{"data":{"Variable":["sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot"],"value":{"__ndarray__":"tQKjeIIVd0Do7CdydF51QGdijk899nhAaBKk2Jo5fkDHKCTbN2KBQPETFqRSxINATx33UCbNhUA=","dtype":"float64","order":"little","shape":[7]},"year":[2010,2011,2012,2013,2014,2015,2016]},"selected":{"id":"2226"},"selection_policy":{"id":"2240"}},"id":"2225","type":"ColumnDataSource"},{"attributes":{},"id":"2226","type":"Selection"},{"attributes":{},"id":"2247","type":"Selection"},{"attributes":{"coordinates":null,"data_source":{"id":"2225"},"glyph":{"id":"2228"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2230"},"nonselection_glyph":{"id":"2229"},"selection_glyph":{"id":"2245"},"view":{"id":"2232"}},"id":"2231","type":"GlyphRenderer"},{"attributes":{"callback":null,"renderers":[{"id":"2231"},{"id":"2252"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["year","@{year}"],["value","@{value}"]]},"id":"2183","type":"HoverTool"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer05253","sizing_mode":"stretch_width"},"id":"2178","type":"Spacer"},{"attributes":{"axis_label":"year","coordinates":null,"formatter":{"id":"2220"},"group":null,"major_label_policy":{"id":"2221"},"ticker":{"id":"2197"}},"id":"2196","type":"LinearAxis"},{"attributes":{"end":4794.80965708199,"reset_end":4794.80965708199,"reset_start":-62.9062279018836,"start":-62.9062279018836,"tags":[[["value","value",null]]]},"id":"2182","type":"Range1d"},{"attributes":{"below":[{"id":"2196"}],"center":[{"id":"2199"},{"id":"2203"}],"height":300,"left":[{"id":"2200"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"2231"},{"id":"2252"}],"right":[{"id":"2243"}],"sizing_mode":"fixed","title":{"id":"2188"},"toolbar":{"id":"2210"},"width":700,"x_range":{"id":"2181"},"x_scale":{"id":"2192"},"y_range":{"id":"2182"},"y_scale":{"id":"2194"}},"id":"2187","subtype":"Figure","type":"Plot"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2230","type":"Line"},{"attributes":{},"id":"2201","type":"BasicTicker"},{"attributes":{},"id":"2197","type":"BasicTicker"},{"attributes":{"label":{"value":"sale_price_sqr_foot"},"renderers":[{"id":"2231"}]},"id":"2244","type":"LegendItem"},{"attributes":{},"id":"2194","type":"LinearScale"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2251","type":"Line"},{"attributes":{},"id":"2220","type":"BasicTickFormatter"}],"root_ids":["2177"]},"title":"Bokeh Application","version":"2.4.1"}};
    var render_items = [{"docid":"53180ae3-849c-4965-8723-09a2652e8e28","root_ids":["2177"],"roots":{"2177":"a7589aa4-39bc-4977-9b01-6888458d81ec"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



### Step 6: Use both the `prices_square_foot_by_year` DataFrame and interactive plots to answer the following questions:

**Question:** Did any year experience a drop in the average sale price per square foot compared to the previous year?

**Answer:** Yes it looks like 2010 leading up to 2011 experienced a slight drop in price per square foot.  Besides that, all the years after 2010 experienced a general increase in price per square foot.

**Question:** If so, did the gross rent increase or decrease during that year?

**Answer:** During 2010, the gross rent price did increase

---

## Compare the Average Sale Prices by Neighborhood

For this part of the assignment, use interactive visualizations and widgets to explore the average sale price per square foot by neighborhood. To do so, complete the following steps:

1. Create a new DataFrame that groups the original DataFrame by year and neighborhood. Aggregate the results by the `mean` of the groups.

2. Filter out the “housing_units” column to create a DataFrame that includes only the `sale_price_sqr_foot` and `gross_rent` averages per year.

3. Create an interactive line plot with hvPlot that visualizes both `sale_price_sqr_foot` and `gross_rent`. Set the x-axis parameter to the year (`x="year"`). Use the `groupby` parameter to create an interactive widget for `neighborhood`.

4. Style and format the line plot to ensure a professionally styled visualization.

5. Note that your resulting plot should appear similar to the following image:

![A screenshot depicts an example of the resulting plot.](Images/pricing-info-by-neighborhood.png)

6. Use the interactive visualization to answer the following question:

    * For the Anza Vista neighborhood, is the average sale price per square foot for 2016 more or less than the price that’s listed for 2012? 


### Step 1: Create a new DataFrame that groups the original DataFrame by year and neighborhood. Aggregate the results by the `mean` of the groups.


```python
# Group by year and neighborhood and then create a new dataframe of the mean values
prices_by_year_by_neighborhood = sfo_data_df.groupby(by=['year', 'neighborhood']).mean()

# Review the DataFrame
prices_by_year_by_neighborhood
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
      <th></th>
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
    <tr>
      <th>year</th>
      <th>neighborhood</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">2010</th>
      <th>Alamo Square</th>
      <td>291.182945</td>
      <td>372560</td>
      <td>1239</td>
    </tr>
    <tr>
      <th>Anza Vista</th>
      <td>267.932583</td>
      <td>372560</td>
      <td>1239</td>
    </tr>
    <tr>
      <th>Bayview</th>
      <td>170.098665</td>
      <td>372560</td>
      <td>1239</td>
    </tr>
    <tr>
      <th>Buena Vista Park</th>
      <td>347.394919</td>
      <td>372560</td>
      <td>1239</td>
    </tr>
    <tr>
      <th>Central Richmond</th>
      <td>319.027623</td>
      <td>372560</td>
      <td>1239</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">2016</th>
      <th>Telegraph Hill</th>
      <td>903.049771</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>Twin Peaks</th>
      <td>970.085470</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>Van Ness/ Civic Center</th>
      <td>552.602567</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>Visitacion Valley</th>
      <td>328.319007</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>Westwood Park</th>
      <td>631.195426</td>
      <td>384242</td>
      <td>4390</td>
    </tr>
  </tbody>
</table>
<p>397 rows × 3 columns</p>
</div>



### Step 2: Filter out the “housing_units” column to create a DataFrame that includes only the `sale_price_sqr_foot` and `gross_rent` averages per year.


```python
# Filter out the housing_units
prices_by_year_by_neighborhood = prices_by_year_by_neighborhood.drop(columns=['housing_units'])

# Review the first and last five rows of the DataFrame
prices_by_year_by_neighborhood.head()
prices_by_year_by_neighborhood.tail()
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
      <th></th>
      <th>sale_price_sqr_foot</th>
      <th>gross_rent</th>
    </tr>
    <tr>
      <th>year</th>
      <th>neighborhood</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">2016</th>
      <th>Telegraph Hill</th>
      <td>903.049771</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>Twin Peaks</th>
      <td>970.085470</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>Van Ness/ Civic Center</th>
      <td>552.602567</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>Visitacion Valley</th>
      <td>328.319007</td>
      <td>4390</td>
    </tr>
    <tr>
      <th>Westwood Park</th>
      <td>631.195426</td>
      <td>4390</td>
    </tr>
  </tbody>
</table>
</div>



### Step 3: Create an interactive line plot with hvPlot that visualizes both `sale_price_sqr_foot` and `gross_rent`. Set the x-axis parameter to the year (`x="year"`). Use the `groupby` parameter to create an interactive widget for `neighborhood`.

### Step 4: Style and format the line plot to ensure a professionally styled visualization.


```python
# Use hvplot to create an interactive line plot of the average price per square foot
#EXAMPLE: average_total_payments_by_state.hvplot.bar(
    #groupby="Provider State", 
    #rot=90, 
    #size=(35, 7)
#)
# The plot should have a dropdown selector for the neighborhood
prices_by_year_by_neighborhood.hvplot(
    groupby="neighborhood",
    title="Sales Prices & Gross Rent per Neighborhood.", 
    x="year", 
    rot=(60),
    figsize=(20,10)
)
```






<div id='2438'>





  <div class="bk-root" id="e6e3e283-24d4-4b5a-a7cb-65bd13057f92" data-root-id="2438"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"136b94ac-b0f9-4956-afa4-de154ad04b2b":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"2465","type":"SaveTool"},{"attributes":{"margin":[5,5,5,5],"name":"VSpacer05628","sizing_mode":"stretch_height"},"id":"2650","type":"Spacer"},{"attributes":{"tools":[{"id":"2444"},{"id":"2465"},{"id":"2466"},{"id":"2467"},{"id":"2468"},{"id":"2469"}]},"id":"2471","type":"Toolbar"},{"attributes":{"end":2016.0,"reset_end":2016.0,"reset_start":2010.0,"start":2010.0,"tags":[[["year","year",null]]]},"id":"2442","type":"Range1d"},{"attributes":{"data":{"Variable":["sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot","sale_price_sqr_foot"],"value":{"__ndarray__":"DUc7WO0yckCafszcbwhxQIKRs5ot42ZAKDog0LQ8eEAVMinKGEd+QI4V5FDt0IJAVCHcmLVPdUA=","dtype":"float64","order":"little","shape":[7]},"year":[2010,2011,2012,2013,2014,2015,2016]},"selected":{"id":"2487"},"selection_policy":{"id":"2501"}},"id":"2486","type":"ColumnDataSource"},{"attributes":{},"id":"2462","type":"BasicTicker"},{"attributes":{},"id":"2455","type":"LinearScale"},{"attributes":{"data":{"Variable":["gross_rent","gross_rent","gross_rent","gross_rent","gross_rent","gross_rent","gross_rent"],"value":[1239,1530,2324,2971,3528,3739,4390],"year":[2010,2011,2012,2013,2014,2015,2016]},"selected":{"id":"2508"},"selection_policy":{"id":"2524"}},"id":"2507","type":"ColumnDataSource"},{"attributes":{"label":{"value":"gross_rent"},"renderers":[{"id":"2513"}]},"id":"2527","type":"LegendItem"},{"attributes":{"label":{"value":"sale_price_sqr_foot"},"renderers":[{"id":"2492"}]},"id":"2505","type":"LegendItem"},{"attributes":{"margin":[20,20,20,20],"min_width":250,"options":["Alamo Square","Anza Vista","Bayview","Buena Vista Park","Central Richmond","Central Sunset","Corona Heights","Cow Hollow","Croker Amazon","Diamond Heights","Downtown ","Eureka Valley/Dolores Heights","Excelsior","Financial District North","Financial District South","Forest Knolls","Glen Park","Golden Gate Heights","Haight Ashbury","Hayes Valley","Hunters Point","Ingleside ","Inner Mission","Inner Parkside","Inner Richmond","Inner Sunset","Jordan Park/Laurel Heights","Lake --The Presidio","Lone Mountain","Lower Pacific Heights","Marina","Miraloma Park","Mission Bay","Mission Dolores","Mission Terrace","Nob Hill","Noe Valley","Oceanview","Outer Parkside","Outer Richmond ","Outer Sunset","Pacific Heights","Park North","Parkside","Parnassus/Ashbury Heights","Portola","Potrero Hill","Presidio Heights","Russian Hill","South Beach","South of Market","Sunnyside","Telegraph Hill","Twin Peaks","Union Square District","Van Ness/ Civic Center","West Portal","Western Addition","Yerba Buena","Bernal Heights ","Clarendon Heights","Duboce Triangle","Ingleside Heights","North Beach","North Waterfront","Outer Mission","Westwood Highlands","Merced Heights","Midtown Terrace","Visitacion Valley","Silver Terrace","Westwood Park","Bayview Heights"],"title":"neighborhood","value":"Alamo Square","width":250},"id":"2649","type":"Select"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2490","type":"Line"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2470","type":"BoxAnnotation"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2512","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"2507"},"glyph":{"id":"2510"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2512"},"nonselection_glyph":{"id":"2511"},"selection_glyph":{"id":"2528"},"view":{"id":"2514"}},"id":"2513","type":"GlyphRenderer"},{"attributes":{"children":[{"id":"2439"},{"id":"2448"},{"id":"2645"},{"id":"2646"}],"margin":[0,0,0,0],"name":"Row05621"},"id":"2438","type":"Row"},{"attributes":{"axis_label":"year","coordinates":null,"formatter":{"id":"2481"},"group":null,"major_label_orientation":1.0471975511965976,"major_label_policy":{"id":"2482"},"ticker":{"id":"2458"}},"id":"2457","type":"LinearAxis"},{"attributes":{"source":{"id":"2507"}},"id":"2514","type":"CDSView"},{"attributes":{"end":4810.690068306854,"reset_end":4810.690068306854,"reset_start":-237.59075137539725,"start":-237.59075137539725,"tags":[[["value","value",null]]]},"id":"2443","type":"Range1d"},{"attributes":{"line_color":"#fc4f30","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2528","type":"Line"},{"attributes":{"line_color":"#30a2da","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2506","type":"Line"},{"attributes":{"children":[{"id":"2647"},{"id":"2648"},{"id":"2650"}],"margin":[0,0,0,0],"name":"Column05629"},"id":"2646","type":"Column"},{"attributes":{},"id":"2524","type":"UnionRenderers"},{"attributes":{"client_comm_id":"15435186449f49c2bc1738aaa2779403","comm_id":"dc917e8d33a746889bf2cfb498e9c774","plot_id":"2438"},"id":"2687","type":"panel.models.comm_manager.CommManager"},{"attributes":{"line_color":"#fc4f30","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2510","type":"Line"},{"attributes":{},"id":"2466","type":"PanTool"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer05630","sizing_mode":"stretch_width"},"id":"2439","type":"Spacer"},{"attributes":{"axis_label":"","coordinates":null,"formatter":{"id":"2484"},"group":null,"major_label_policy":{"id":"2485"},"ticker":{"id":"2462"}},"id":"2461","type":"LinearAxis"},{"attributes":{},"id":"2458","type":"BasicTicker"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2491","type":"Line"},{"attributes":{"axis":{"id":"2457"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2460","type":"Grid"},{"attributes":{"coordinates":null,"data_source":{"id":"2486"},"glyph":{"id":"2489"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2491"},"nonselection_glyph":{"id":"2490"},"selection_glyph":{"id":"2506"},"view":{"id":"2493"}},"id":"2492","type":"GlyphRenderer"},{"attributes":{},"id":"2467","type":"WheelZoomTool"},{"attributes":{},"id":"2481","type":"BasicTickFormatter"},{"attributes":{},"id":"2453","type":"LinearScale"},{"attributes":{"coordinates":null,"group":null,"text":"Sales Prices & Gross Rent per Neighborhood.","text_color":"black","text_font_size":"12pt"},"id":"2449","type":"Title"},{"attributes":{"line_color":"#30a2da","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2489","type":"Line"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2505"},{"id":"2527"}],"location":[0,0],"title":"Variable"},"id":"2504","type":"Legend"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer05631","sizing_mode":"stretch_width"},"id":"2645","type":"Spacer"},{"attributes":{"overlay":{"id":"2470"}},"id":"2468","type":"BoxZoomTool"},{"attributes":{},"id":"2484","type":"BasicTickFormatter"},{"attributes":{"margin":[5,5,5,5],"name":"VSpacer05627","sizing_mode":"stretch_height"},"id":"2647","type":"Spacer"},{"attributes":{},"id":"2501","type":"UnionRenderers"},{"attributes":{},"id":"2487","type":"Selection"},{"attributes":{"children":[{"id":"2649"}],"css_classes":["panel-widget-box"],"margin":[5,5,5,5],"name":"WidgetBox05622"},"id":"2648","type":"Column"},{"attributes":{"source":{"id":"2486"}},"id":"2493","type":"CDSView"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"x":{"field":"year"},"y":{"field":"value"}},"id":"2511","type":"Line"},{"attributes":{},"id":"2485","type":"AllLabels"},{"attributes":{"below":[{"id":"2457"}],"center":[{"id":"2460"},{"id":"2464"}],"height":300,"left":[{"id":"2461"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"2492"},{"id":"2513"}],"right":[{"id":"2504"}],"sizing_mode":"fixed","title":{"id":"2449"},"toolbar":{"id":"2471"},"width":700,"x_range":{"id":"2442"},"x_scale":{"id":"2453"},"y_range":{"id":"2443"},"y_scale":{"id":"2455"}},"id":"2448","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"2508","type":"Selection"},{"attributes":{},"id":"2482","type":"AllLabels"},{"attributes":{"callback":null,"renderers":[{"id":"2492"},{"id":"2513"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["year","@{year}"],["value","@{value}"]]},"id":"2444","type":"HoverTool"},{"attributes":{},"id":"2469","type":"ResetTool"},{"attributes":{"axis":{"id":"2461"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2464","type":"Grid"}],"root_ids":["2438","2687"]},"title":"Bokeh Application","version":"2.4.1"}};
    var render_items = [{"docid":"136b94ac-b0f9-4956-afa4-de154ad04b2b","root_ids":["2438"],"roots":{"2438":"e6e3e283-24d4-4b5a-a7cb-65bd13057f92"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



### Step 6: Use the interactive visualization to answer the following question:

**Question:** For the Anza Vista neighborhood, is the average sale price per square foot for 2016 more or less than the price that’s listed for 2012? 

**Answer:** The price per square foot in 2016 is less than in 2012, however the gross rent is much higher than in 2012.

---

## Build an Interactive Neighborhood Map

For this part of the assignment, explore the geospatial relationships in the data by using interactive visualizations with hvPlot and GeoViews. To build your map, use the `sfo_data_df` DataFrame (created during the initial import), which includes the neighborhood location data with the average prices. To do all this, complete the following steps:

1. Read the `neighborhood_coordinates.csv` file from the `Resources` folder into the notebook, and create a DataFrame named `neighborhood_locations_df`. Be sure to set the `index_col` of the DataFrame as “Neighborhood”.

2. Using the original `sfo_data_df` Dataframe, create a DataFrame named `all_neighborhood_info_df` that groups the data by neighborhood. Aggregate the results by the `mean` of the group.

3. Review the two code cells that concatenate the `neighborhood_locations_df` DataFrame with the `all_neighborhood_info_df` DataFrame. Note that the first cell uses the [Pandas concat function](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html) to create a DataFrame named `all_neighborhoods_df`. The second cell cleans the data and sets the “Neighborhood” column. Be sure to run these cells to create the `all_neighborhoods_df` DataFrame, which you’ll need to create the geospatial visualization.

4. Using hvPlot with GeoViews enabled, create a `points` plot for the `all_neighborhoods_df` DataFrame. Be sure to do the following:

    * Set the `size` parameter to “sale_price_sqr_foot”.

    * Set the `color` parameter to “gross_rent”.

    * Set the `size_max` parameter to “25”.

    * Set the `zoom` parameter to “11”.

Note that your resulting plot should appear similar to the following image:

![A screenshot depicts an example of a scatter plot created with hvPlot and GeoViews.](Images/6-4-geoviews-plot.png)

5. Use the interactive map to answer the following question:

    * Which neighborhood has the highest gross rent, and which has the highest sale price per square foot?


### Step 1: Read the `neighborhood_coordinates.csv` file from the `Resources` folder into the notebook, and create a DataFrame named `neighborhood_locations_df`. Be sure to set the `index_col` of the DataFrame as “Neighborhood”.


```python
# Load neighborhoods coordinates data
neighborhood_locations_df = pd.read_csv(
    Path("../Starter_Code/neighborhoods_coordinates.csv"),
    index_col="Neighborhood"
)
# Review the DataFrame
neighborhood_locations_df
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
      <th>Lat</th>
      <th>Lon</th>
    </tr>
    <tr>
      <th>Neighborhood</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alamo Square</th>
      <td>37.791012</td>
      <td>-122.402100</td>
    </tr>
    <tr>
      <th>Anza Vista</th>
      <td>37.779598</td>
      <td>-122.443451</td>
    </tr>
    <tr>
      <th>Bayview</th>
      <td>37.734670</td>
      <td>-122.401060</td>
    </tr>
    <tr>
      <th>Bayview Heights</th>
      <td>37.728740</td>
      <td>-122.410980</td>
    </tr>
    <tr>
      <th>Bernal Heights</th>
      <td>37.728630</td>
      <td>-122.443050</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>West Portal</th>
      <td>37.740260</td>
      <td>-122.463880</td>
    </tr>
    <tr>
      <th>Western Addition</th>
      <td>37.792980</td>
      <td>-122.435790</td>
    </tr>
    <tr>
      <th>Westwood Highlands</th>
      <td>37.734700</td>
      <td>-122.456854</td>
    </tr>
    <tr>
      <th>Westwood Park</th>
      <td>37.734150</td>
      <td>-122.457000</td>
    </tr>
    <tr>
      <th>Yerba Buena</th>
      <td>37.792980</td>
      <td>-122.396360</td>
    </tr>
  </tbody>
</table>
<p>73 rows × 2 columns</p>
</div>



### Step 2: Using the original `sfo_data_df` Dataframe, create a DataFrame named `all_neighborhood_info_df` that groups the data by neighborhood. Aggregate the results by the `mean` of the group.


```python
# Calculate the mean values for each neighborhood
all_neighborhood_info_df = sfo_data_df.groupby(by="neighborhood").mean()

# Review the resulting DataFrame
all_neighborhood_info_df
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
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
    <tr>
      <th>neighborhood</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alamo Square</th>
      <td>366.020712</td>
      <td>378401.00</td>
      <td>2817.285714</td>
    </tr>
    <tr>
      <th>Anza Vista</th>
      <td>373.382198</td>
      <td>379050.00</td>
      <td>3031.833333</td>
    </tr>
    <tr>
      <th>Bayview</th>
      <td>204.588623</td>
      <td>376454.00</td>
      <td>2318.400000</td>
    </tr>
    <tr>
      <th>Bayview Heights</th>
      <td>590.792839</td>
      <td>382295.00</td>
      <td>3739.000000</td>
    </tr>
    <tr>
      <th>Bernal Heights</th>
      <td>576.746488</td>
      <td>379374.50</td>
      <td>3080.333333</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>West Portal</th>
      <td>498.488485</td>
      <td>376940.75</td>
      <td>2515.500000</td>
    </tr>
    <tr>
      <th>Western Addition</th>
      <td>307.562201</td>
      <td>377427.50</td>
      <td>2555.166667</td>
    </tr>
    <tr>
      <th>Westwood Highlands</th>
      <td>533.703935</td>
      <td>376454.00</td>
      <td>2250.500000</td>
    </tr>
    <tr>
      <th>Westwood Park</th>
      <td>687.087575</td>
      <td>382295.00</td>
      <td>3959.000000</td>
    </tr>
    <tr>
      <th>Yerba Buena</th>
      <td>576.709848</td>
      <td>377427.50</td>
      <td>2555.166667</td>
    </tr>
  </tbody>
</table>
<p>73 rows × 3 columns</p>
</div>



### Step 3: Review the two code cells that concatenate the `neighborhood_locations_df` DataFrame with the `all_neighborhood_info_df` DataFrame. 

Note that the first cell uses the [Pandas concat function](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html) to create a DataFrame named `all_neighborhoods_df`. 

The second cell cleans the data and sets the “Neighborhood” column. 

Be sure to run these cells to create the `all_neighborhoods_df` DataFrame, which you’ll need to create the geospatial visualization.


```python
# Using the Pandas `concat` function, join the 
# neighborhood_locations_df and the all_neighborhood_info_df DataFrame
# The axis of the concatenation is "columns".
# The concat function will automatially combine columns with
# identical information, while keeping the additional columns.
all_neighborhoods_df = pd.concat(
    [neighborhood_locations_df, all_neighborhood_info_df], 
    axis="columns",
    sort=False
)

# Review the resulting DataFrame
display(all_neighborhoods_df.head())
display(all_neighborhoods_df.tail())

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
      <th>Lat</th>
      <th>Lon</th>
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alamo Square</th>
      <td>37.791012</td>
      <td>-122.402100</td>
      <td>366.020712</td>
      <td>378401.0</td>
      <td>2817.285714</td>
    </tr>
    <tr>
      <th>Anza Vista</th>
      <td>37.779598</td>
      <td>-122.443451</td>
      <td>373.382198</td>
      <td>379050.0</td>
      <td>3031.833333</td>
    </tr>
    <tr>
      <th>Bayview</th>
      <td>37.734670</td>
      <td>-122.401060</td>
      <td>204.588623</td>
      <td>376454.0</td>
      <td>2318.400000</td>
    </tr>
    <tr>
      <th>Bayview Heights</th>
      <td>37.728740</td>
      <td>-122.410980</td>
      <td>590.792839</td>
      <td>382295.0</td>
      <td>3739.000000</td>
    </tr>
    <tr>
      <th>Bernal Heights</th>
      <td>37.728630</td>
      <td>-122.443050</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



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
      <th>Lat</th>
      <th>Lon</th>
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Yerba Buena</th>
      <td>37.79298</td>
      <td>-122.39636</td>
      <td>576.709848</td>
      <td>377427.5</td>
      <td>2555.166667</td>
    </tr>
    <tr>
      <th>Bernal Heights</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>576.746488</td>
      <td>379374.5</td>
      <td>3080.333333</td>
    </tr>
    <tr>
      <th>Downtown</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>391.434378</td>
      <td>378401.0</td>
      <td>2817.285714</td>
    </tr>
    <tr>
      <th>Ingleside</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>367.895144</td>
      <td>377427.5</td>
      <td>2509.000000</td>
    </tr>
    <tr>
      <th>Outer Richmond</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>473.900773</td>
      <td>378401.0</td>
      <td>2817.285714</td>
    </tr>
  </tbody>
</table>
</div>



```python
# Call the dropna function to remove any neighborhoods that do not have data
all_neighborhoods_df = all_neighborhoods_df.reset_index().dropna()

# Rename the "index" column as "Neighborhood" for use in the Visualization
all_neighborhoods_df = all_neighborhoods_df.rename(columns={"index": "Neighborhood"})

# Review the resulting DataFrame
display(all_neighborhoods_df.head())
display(all_neighborhoods_df.tail())
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
      <th>Neighborhood</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alamo Square</td>
      <td>37.791012</td>
      <td>-122.402100</td>
      <td>366.020712</td>
      <td>378401.0</td>
      <td>2817.285714</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Anza Vista</td>
      <td>37.779598</td>
      <td>-122.443451</td>
      <td>373.382198</td>
      <td>379050.0</td>
      <td>3031.833333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bayview</td>
      <td>37.734670</td>
      <td>-122.401060</td>
      <td>204.588623</td>
      <td>376454.0</td>
      <td>2318.400000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bayview Heights</td>
      <td>37.728740</td>
      <td>-122.410980</td>
      <td>590.792839</td>
      <td>382295.0</td>
      <td>3739.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Buena Vista Park</td>
      <td>37.768160</td>
      <td>-122.439330</td>
      <td>452.680591</td>
      <td>378076.5</td>
      <td>2698.833333</td>
    </tr>
  </tbody>
</table>
</div>



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
      <th>Neighborhood</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>sale_price_sqr_foot</th>
      <th>housing_units</th>
      <th>gross_rent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>68</th>
      <td>West Portal</td>
      <td>37.74026</td>
      <td>-122.463880</td>
      <td>498.488485</td>
      <td>376940.75</td>
      <td>2515.500000</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Western Addition</td>
      <td>37.79298</td>
      <td>-122.435790</td>
      <td>307.562201</td>
      <td>377427.50</td>
      <td>2555.166667</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Westwood Highlands</td>
      <td>37.73470</td>
      <td>-122.456854</td>
      <td>533.703935</td>
      <td>376454.00</td>
      <td>2250.500000</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Westwood Park</td>
      <td>37.73415</td>
      <td>-122.457000</td>
      <td>687.087575</td>
      <td>382295.00</td>
      <td>3959.000000</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Yerba Buena</td>
      <td>37.79298</td>
      <td>-122.396360</td>
      <td>576.709848</td>
      <td>377427.50</td>
      <td>2555.166667</td>
    </tr>
  </tbody>
</table>
</div>


### Step 4: Using hvPlot with GeoViews enabled, create a `points` plot for the `all_neighborhoods_df` DataFrame. Be sure to do the following:

* Set the `geo` parameter to True.
* Set the `size` parameter to “sale_price_sqr_foot”.
* Set the `color` parameter to “gross_rent”.
* Set the `frame_width` parameter to 700.
* Set the `frame_height` parameter to 500.
* Include a descriptive title.


```python
# Create a plot to analyze neighborhood info
all_neighborhoods_df.hvplot.points(
    "Lon",
    "Lat",
    geo=True,
    size="sale_price_sqr_foot",
    color="gross_rent",
    tiles='OSM',
    hover_cols=['Neighborhood', 'gross_rent'],
    frame_width=700,
    frame_height=500,
    title="Neighborhood Information Analysis of Gross Rent and Price per Sqr Foot"
)

```






<div id='2748'>





  <div class="bk-root" id="2563a670-c0a2-4c13-b8fe-b323b73374e7" data-root-id="2748"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"ede503f3-3623-4a9f-8f57-87d990f1b2f0":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"2782","type":"ResetTool"},{"attributes":{},"id":"2806","type":"AllLabels"},{"attributes":{"fill_color":{"field":"color","transform":{"id":"2824"}},"hatch_color":{"field":"color","transform":{"id":"2824"}},"line_color":{"field":"color","transform":{"id":"2824"}},"size":{"field":"size"},"x":{"field":"Lon"},"y":{"field":"Lat"}},"id":"2830","type":"Scatter"},{"attributes":{},"id":"2799","type":"AllLabels"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer06305","sizing_mode":"stretch_width"},"id":"2749","type":"Spacer"},{"attributes":{"high":3959.0,"low":1781.5,"palette":["#b3fef5","#b0fef5","#adfdf5","#a9fcf5","#a6fbf6","#a3faf6","#a0faf6","#9df9f6","#9af8f6","#97f7f6","#93f7f6","#90f6f6","#8df5f6","#8af4f7","#87f3f7","#83f2f7","#80f2f7","#7df1f7","#79f0f7","#76eff7","#73eef7","#6fedf8","#6cecf8","#68ecf8","#65ebf8","#61eaf8","#5ee9f8","#5ae8f8","#57e7f8","#53e6f8","#50e5f9","#4ce4f9","#49e3f9","#45e2f9","#42e1f9","#3ee0f9","#3bdff9","#38def9","#35ddf9","#32dcf9","#30dbfa","#2ed9fa","#2dd8fa","#2cd7fa","#2bd6fa","#2bd5fa","#2ad3fa","#2ad2fa","#29d1fa","#29d0fb","#29cffb","#28cdfb","#28ccfb","#28cbfb","#28cafb","#28c8fb","#28c7fb","#29c6fb","#29c5fb","#29c4fb","#29c2fb","#2ac1fb","#2ac0fb","#2bbffb","#2bbdfc","#2cbcfc","#2dbbfc","#2db9fc","#2eb8fc","#2fb7fc","#2fb6fc","#30b4fc","#31b3fc","#32b2fc","#32b0fc","#33affc","#33aefc","#34adfc","#34abfc","#34aafc","#35a9fc","#35a8fc","#35a6fc","#35a5fc","#35a4fc","#35a3fc","#35a1fc","#35a0fc","#359ffc","#359dfc","#359cfc","#359bfc","#349afd","#3498fd","#3497fd","#3396fd","#3395fd","#3293fd","#3292fd","#3191fd","#3090fd","#308ffd","#2f8dfd","#2f8cfd","#2e8bfd","#2e8afd","#2d88fd","#2d87fd","#2c86fd","#2c84fd","#2c83fd","#2c82fd","#2b81fd","#2b7ffd","#2b7efd","#2b7dfd","#2b7bfd","#2b7afd","#2b79fd","#2b77fd","#2b76fd","#2b75fd","#2b73fd","#2c72fd","#2c71fd","#2c6ffd","#2c6efd","#2d6cfd","#2d6bfd","#2d6afc","#2e68fc","#2e67fc","#2e65fc","#2e64fc","#2f62fc","#2f61fc","#2f5ffc","#2f5efc","#2f5dfc","#2f5bfc","#2f5afc","#2f58fb","#2f57fb","#2f55fb","#2f53fb","#2f52fb","#2f50fb","#2f4ffb","#2f4dfb","#2e4cfb","#2e4afb","#2e48fb","#2e47fa","#2d45fa","#2d43fa","#2d42fa","#2d40fa","#2c3efa","#2c3dfa","#2b3bf9","#2b39f9","#2a37f9","#2a36f8","#2934f8","#2832f7","#2831f7","#272ff6","#262ef5","#252cf5","#252af4","#2429f3","#2327f2","#2226f1","#2124f0","#2023ef","#1f22ee","#1e20ed","#1d1feb","#1c1eea","#1b1ce9","#1a1be7","#181ae6","#1719e5","#1618e3","#1417e1","#1316e0","#1215de","#1014dc","#0f13db","#0e12d9","#0d11d7","#0c10d5","#0b0fd3","#0a0ed1","#090dd0","#080dce","#080ccc","#070bca","#070ac8","#0709c6","#0708c4","#0707c2","#0707bf","#0806bd","#0806bb","#0905b9","#0904b7","#0a04b5","#0a04b2","#0b03b0","#0c03ae","#0d02ab","#0e02a9","#0e02a7","#0f02a4","#0f01a2","#1001a0","#10019d","#10019b","#100199","#100197","#100194","#0f0192","#0f0190","#0f018e","#0e018b","#0e0189","#0d0187","#0d0185","#0c0183","#0b0181","#0b017e","#0a017c","#09017a","#090178","#080276","#070274","#060272","#060270","#05026e","#04026c","#030269","#030267","#020265","#010263","#010261","#00025f","#00025d","#00025b","#000259","#000257","#000255","#000154","#000152","#000150","#00004e"]},"id":"2824","type":"LinearColorMapper"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"field":"color","transform":{"id":"2824"}},"hatch_alpha":{"value":0.2},"hatch_color":{"field":"color","transform":{"id":"2824"}},"line_alpha":{"value":0.2},"line_color":{"field":"color","transform":{"id":"2824"}},"size":{"field":"size"},"x":{"field":"Lon"},"y":{"field":"Lat"}},"id":"2832","type":"Scatter"},{"attributes":{"callback":null,"formatters":{"$x":{"id":"2837"},"$y":{"id":"2838"}},"renderers":[{"id":"2833"}],"tags":["hv_created"],"tooltips":[["Lon","$x{custom}"],["Lat","$y{custom}"],["gross_rent","@{gross_rent}"],["sale_price_sqr_foot","@{sale_price_sqr_foot}"],["Neighborhood","@{Neighborhood}"]]},"id":"2758","type":"HoverTool"},{"attributes":{},"id":"2835","type":"BasicTicker"},{"attributes":{"end":4552573.750035822,"min_interval":5,"reset_end":4552573.750035822,"reset_start":4538787.829534142,"start":4538787.829534142,"tags":[[["Lat","Lat",null]]]},"id":"2755","type":"Range1d"},{"attributes":{"coordinates":null,"group":null,"text":"Neighborhood Information Analysis of Gross Rent and Price per Sqr Foot","text_color":"black","text_font_size":"12pt"},"id":"2762","type":"Title"},{"attributes":{},"id":"2766","type":"LinearScale"},{"attributes":{"children":[{"id":"2749"},{"id":"2761"},{"id":"2902"}],"margin":[0,0,0,0],"name":"Row06301","tags":["embedded"]},"id":"2748","type":"Row"},{"attributes":{},"id":"2843","type":"UnionRenderers"},{"attributes":{"end":-13619293.631218664,"min_interval":5,"reset_end":-13619293.631218664,"reset_start":-13638593.919921018,"start":-13638593.919921018,"tags":[[["Lon","Lon",null]]]},"id":"2754","type":"Range1d"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"field":"color","transform":{"id":"2824"}},"hatch_alpha":{"value":0.1},"hatch_color":{"field":"color","transform":{"id":"2824"}},"line_alpha":{"value":0.1},"line_color":{"field":"color","transform":{"id":"2824"}},"size":{"field":"size"},"x":{"field":"Lon"},"y":{"field":"Lat"}},"id":"2829","type":"Scatter"},{"attributes":{},"id":"2779","type":"PanTool"},{"attributes":{"below":[{"id":"2770"}],"center":[{"id":"2773"},{"id":"2777"}],"frame_height":500,"frame_width":700,"height":null,"left":[{"id":"2774"}],"margin":[5,5,5,5],"match_aspect":true,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"2822"},{"id":"2833"}],"right":[{"id":"2836"}],"sizing_mode":"fixed","title":{"id":"2762"},"toolbar":{"id":"2784"},"width":null,"x_range":{"id":"2754"},"x_scale":{"id":"2766"},"y_range":{"id":"2755"},"y_scale":{"id":"2768"}},"id":"2761","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"2768","type":"LinearScale"},{"attributes":{"source":{"id":"2825"}},"id":"2834","type":"CDSView"},{"attributes":{},"id":"2826","type":"Selection"},{"attributes":{"code":"\n        var projections = Bokeh.require(\"core/util/projections\");\n        var x = special_vars.data_x\n        var y = special_vars.data_y\n        if (projections.wgs84_mercator.invert == null) {\n          var coords = projections.wgs84_mercator.inverse([x, y])\n        } else {\n          var coords = projections.wgs84_mercator.invert(x, y)\n        }\n        return \"\" + (coords[0]).toFixed(4)\n    "},"id":"2837","type":"CustomJSHover"},{"attributes":{"code":"\n        var projections = Bokeh.require(\"core/util/projections\");\n        var x = special_vars.data_x\n        var y = special_vars.data_y\n        if (projections.wgs84_mercator.invert == null) {\n          var coords = projections.wgs84_mercator.inverse([x, y])\n        } else {\n          var coords = projections.wgs84_mercator.invert(x, y)\n        }\n        return \"\" + (coords[1]).toFixed(4)\n    "},"id":"2838","type":"CustomJSHover"},{"attributes":{"axis":{"id":"2770"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2773","type":"Grid"},{"attributes":{"margin":[5,5,5,5],"name":"HSpacer06306","sizing_mode":"stretch_width"},"id":"2902","type":"Spacer"},{"attributes":{"dimension":"lat"},"id":"2795","type":"MercatorTicker"},{"attributes":{},"id":"2841","type":"NoOverlap"},{"attributes":{"fill_color":{"field":"color","transform":{"id":"2824"}},"hatch_color":{"field":"color","transform":{"id":"2824"}},"line_color":{"field":"color","transform":{"id":"2824"}},"size":{"field":"size"},"x":{"field":"Lon"},"y":{"field":"Lat"}},"id":"2828","type":"Scatter"},{"attributes":{"attribution":"&copy; <a href=\"https://www.openstreetmap.org/copyright\">OpenStreetMap</a> contributors","url":"https://c.tile.openstreetmap.org/{Z}/{X}/{Y}.png"},"id":"2819","type":"WMTSTileSource"},{"attributes":{"dimension":"lon"},"id":"2794","type":"MercatorTickFormatter"},{"attributes":{"coordinates":null,"data_source":{"id":"2825"},"glyph":{"id":"2828"},"group":null,"hover_glyph":{"id":"2831"},"muted_glyph":{"id":"2832"},"nonselection_glyph":{"id":"2829"},"selection_glyph":{"id":"2830"},"view":{"id":"2834"}},"id":"2833","type":"GlyphRenderer"},{"attributes":{"axis_label":"x","coordinates":null,"formatter":{"id":"2794"},"group":null,"major_label_policy":{"id":"2799"},"ticker":{"id":"2793"}},"id":"2770","type":"LinearAxis"},{"attributes":{"dimension":"lon"},"id":"2793","type":"MercatorTicker"},{"attributes":{"bar_line_color":"black","color_mapper":{"id":"2824"},"coordinates":null,"group":null,"label_standoff":8,"location":[0,0],"major_label_policy":{"id":"2841"},"major_tick_line_color":"black","ticker":{"id":"2835"}},"id":"2836","type":"ColorBar"},{"attributes":{"dimension":"lat"},"id":"2796","type":"MercatorTickFormatter"},{"attributes":{"tools":[{"id":"2758"},{"id":"2778"},{"id":"2779"},{"id":"2780"},{"id":"2781"},{"id":"2782"}]},"id":"2784","type":"Toolbar"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2783","type":"BoxAnnotation"},{"attributes":{},"id":"2778","type":"SaveTool"},{"attributes":{"coordinates":null,"group":null,"level":"glyph","tile_source":{"id":"2819"}},"id":"2822","type":"TileRenderer"},{"attributes":{"axis_label":"y","coordinates":null,"formatter":{"id":"2796"},"group":null,"major_label_policy":{"id":"2806"},"ticker":{"id":"2795"}},"id":"2774","type":"LinearAxis"},{"attributes":{"axis":{"id":"2774"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2777","type":"Grid"},{"attributes":{"data":{"Lat":{"__ndarray__":"DAZSL05bUUERJz5BvFlRQRU76MWOU1FBI8JNG75SUUGFpr+KKVhRQWlQdR2AWVFBHmRUj5xVUUHwUo3KHlZRQamMnCKNWlFBwh7ofZNbUUH1Euga7VFRQYtMcTy6UlFBtgKVpV1YUUEeKP9IbVZRQSPCTRu+UlFBtiBKHU5bUUG2IEodTltRQb9yAcqiVlFBi0xxPLpSUUH2pqnHwFZRQdQYfxjDV1FB1Bh/GMNXUUEhKqF0TFJRQfKTZ2ezUVFBck/MtTxWUUEeZFSPnFVRQbcd1Q5LWFFBHmRUj5xVUUGpjJwijVpRQVISHETAXFFBcdp6raNZUUHCHuh9k1tRQUpmPd6AXFFBuyX+IIhRUUHwUo3KHlZRQXxYeXl8U1FBOpYAJTxaUUEE/sQM61VRQYtMcTy6UlFBwh7ofZNbUUHwUo3KHlZRQQfJ9hmeXFFBB8n2GZ5cUUG7Jf4giFFRQfUS6BrtUVFBVbZHH+ZVUUEeZFSPnFVRQcIe6H2TW1FBAah9cLFSUUEeZFSPnFVRQZ61yz+HV1FBFTvoxY5TUUGqKCppL1NRQamMnCKNWlFBwh7ofZNbUUEVO+jFjlNRQTqWACU8WlFBtiBKHU5bUUF8WHl5fFNRQQfJ9hmeXFFBbxiQ0x5WUUG2IEodTltRQX83Aii5WVFBI8JNG75SUUGvOKR9U1RRQcIe6H2TW1FBQfYp1I9TUUF8WHl5fFNRQcIe6H2TW1FB","dtype":"float64","order":"little","shape":[69]},"Lon":{"__ndarray__":"2Xg1bjH9acE0qLjTcP9pwWIIf/Ui/WnBhWDB/qz9acEnJMx7N/9pwdoRL7+I/2nBLuMMavgBasFhguugov9pwWYMEXIfAGrBzYeGOQb/acHLaPmTLv9pwTUFRT9r/2nB7DUzh3/+acFQEfkNIv9pwYVgwf6s/WnB2Xg1bjH9acHZeDVuMf1pwf1pccgSAGrBNQVFP2v/acFOyBkLjwBqwZGBGpcI/2nBkYEalwj/acF8DJaHi/tpwVVsW8jPAWrBBt4IIiP+acEu4wxq+AFqwR1EUwafAWrBU03OIvgBasFmDBFyHwBqwcIxFowLAGrB8Jta+RAAasHNh4Y5Bv9pwYTIrvq2AGrBU0Ux5qkAasFhguugov9pwSdRSlwtAGrBWQj8QB39acFeKdIkc/5pwTUFRT9r/2nBzYeGOQb/acFhguugov9pwbG993VQ/WnBsb33dVD9acFTRTHmqQBqwcto+ZMu/2nB11zUS5T/acEu4wxq+AFqwc2HhjkG/2nBV+K4q1gBasEu4wxq+AFqwXNXvGc2AGrBYgh/9SL9acF4d6v+KvxpwWYMEXIfAGrBzYeGOQb/acFiCH/1Iv1pwVkI/EAd/WnB2Xg1bjH9acEnUUpcLQBqwbG993VQ/WnBfJRbnaL/acHZeDVuMf1pwZbRs5Et/mnBhWDB/qz9acFSr2MYjQBqwc2HhjkG/2nB0aQ0VCsAasEnUUpcLQBqwW14C4/h/GnB","dtype":"float64","order":"little","shape":[69]},"Neighborhood":["Alamo Square","Anza Vista","Bayview","Bayview Heights","Buena Vista Park","Central Richmond","Central Sunset","Clarendon Heights","Corona Heights","Cow Hollow","Croker Amazon","Diamond Heights","Duboce Triangle","Eureka Valley/Dolores Heights","Excelsior","Financial District North","Financial District South","Forest Knolls","Glen Park","Golden Gate Heights","Haight Ashbury","Hayes Valley","Hunters Point","Ingleside Heights","Inner Mission","Inner Parkside","Inner Richmond","Inner Sunset","Jordan Park/Laurel Heights","Lake --The Presidio","Lone Mountain","Lower Pacific Heights","Marina","Merced Heights","Midtown Terrace","Miraloma Park","Mission Bay","Mission Dolores","Mission Terrace","Nob Hill","Noe Valley","North Beach","North Waterfront","Oceanview","Outer Mission","Outer Parkside","Outer Sunset","Pacific Heights","Park North","Parkside","Parnassus/Ashbury Heights","Portola","Potrero Hill","Presidio Heights","Russian Hill","Silver Terrace","South Beach","South of Market","Sunnyside","Telegraph Hill","Twin Peaks","Union Square District","Van Ness/ Civic Center","Visitacion Valley","West Portal","Western Addition","Westwood Highlands","Westwood Park","Yerba Buena"],"color":{"__ndarray__":"SZIkSZICpkCrqqqqqq+nQM3MzMzMHKJAAAAAAAA2rUCrqqqqqhWlQEmSJEmSAqZASZIkSZICpkAAAAAAAJWhQAAAAAAAUKNASZIkSZICpkCrqqqqqhWlQAAAAAAAgJ9AAAAAAIC4pUBJkiRJkgKmQKuqqqqqr6dASZIkSZICpkAAAAAAAFCjQAAAAAAA1ptAAAAAAACnpkDNzMzMzFKkQEmSJEmSAqZASZIkSZICpkAAAAAAAHKjQAAAAAAAIKdASZIkSZICpkAAAAAAADCpQEmSJEmSAqZASZIkSZICpkBJkiRJkgKmQFVVVVVV9qNAVVVVVVX2o0BJkiRJkgKmQEmSJEmSAqZAAAAAAACsqkAAAAAAAK+kQAAAAACA1qBAVVVVVVXPpEBVVVVVVfajQM3MzMzMyqhASZIkSZICpkBJkiRJkgKmQJqZmZmZXadAzczMzMwEpkAAAAAAAASjQAAAAACAZ6dASZIkSZICpkBJkiRJkgKmQEmSJEmSAqZASZIkSZICpkBVVVVVVfajQEmSJEmSAqZAzczMzMwcokBJkiRJkgKmQEmSJEmSAqZASZIkSZICpkAAAAAAAJCrQAAAAAAAZqBASZIkSZICpkAAAAAAAKemQEmSJEmSAqZASZIkSZICpkBVVVVVVfajQEmSJEmSAqZAAAAAAACSrEAAAAAAAKejQFVVVVVV9qNAAAAAAACVoUAAAAAAAO6uQFVVVVVV9qNA","dtype":"float64","order":"little","shape":[69]},"gross_rent":{"__ndarray__":"SZIkSZICpkCrqqqqqq+nQM3MzMzMHKJAAAAAAAA2rUCrqqqqqhWlQEmSJEmSAqZASZIkSZICpkAAAAAAAJWhQAAAAAAAUKNASZIkSZICpkCrqqqqqhWlQAAAAAAAgJ9AAAAAAIC4pUBJkiRJkgKmQKuqqqqqr6dASZIkSZICpkAAAAAAAFCjQAAAAAAA1ptAAAAAAACnpkDNzMzMzFKkQEmSJEmSAqZASZIkSZICpkAAAAAAAHKjQAAAAAAAIKdASZIkSZICpkAAAAAAADCpQEmSJEmSAqZASZIkSZICpkBJkiRJkgKmQFVVVVVV9qNAVVVVVVX2o0BJkiRJkgKmQEmSJEmSAqZAAAAAAACsqkAAAAAAAK+kQAAAAACA1qBAVVVVVVXPpEBVVVVVVfajQM3MzMzMyqhASZIkSZICpkBJkiRJkgKmQJqZmZmZXadAzczMzMwEpkAAAAAAAASjQAAAAACAZ6dASZIkSZICpkBJkiRJkgKmQEmSJEmSAqZASZIkSZICpkBVVVVVVfajQEmSJEmSAqZAzczMzMwcokBJkiRJkgKmQEmSJEmSAqZASZIkSZICpkAAAAAAAJCrQAAAAAAAZqBASZIkSZICpkAAAAAAAKemQEmSJEmSAqZASZIkSZICpkBVVVVVVfajQEmSJEmSAqZAAAAAAACSrEAAAAAAAKejQFVVVVVV9qNAAAAAAACVoUAAAAAAAO6uQFVVVVVV9qNA","dtype":"float64","order":"little","shape":[69]},"sale_price_sqr_foot":{"__ndarray__":"cJyd1VTgdkAk1Xd7HVZ3QMu7p//VkmlA/23ou1d2gkDg3lCz40p8QJIjOSXCpnhA8UR6wAF7ekBhIJwN63N+QHYgjQJQXIJAC1duW7bPhEAtmqsjEfByQA42LireL3tABPQiJJZpf0DNvwdH/RGEQIsgrDxBTHhATKx878x1eEBGXRUgYX18QOcoDRF0HXRAyP/3Apx+g0Bq5HcBJvuDQEKNqFi3GHxAP5v/3Ow+dkAFP/VX/1NlQPiqSlDEDHhAfmgaicfaeECrA3m3FTuAQN85W0+CqXdAyXixJK7aeUCo3p7PEouAQBsbzgZFn3lA1YI2J6jjfUACLzpc59mAQOp1/4D+PYJAfSK5L8KmiEDjM3wxFTSBQFx/3pp8XohAcr9Bp4mxgUC9Hcljpi56QAabwse6W4BAhQqU+EOjfEAzwd8Vi/OAQA1GhppZunlANoyg6lAkf0A0VzbjjqF0QGKNxdbeS25ATpTGpG5QfkBicP8ngaN4QBWbFVByjIVAMlaax7lbd0AUODg4wwJ1QGCc43LsxYNAk1Rio9JxdECazArhG7CEQMvN+TvNGoVAb73/oN0Hg0DTKbSOXEllQMmt8u7+UIRAh+HC4SvSgUD/YX/xi4KAQLsKhXgNJIVAiyAuxmBWfUAlEBQx8j+MQEdUUCNnQnlAoAgieXXXckDJ9HfV0Cd/QF9rdMb+OHNAQN/mqKGtgEBr0ERas3iFQIOO/MStBYJA","dtype":"float64","order":"little","shape":[69]},"size":{"__ndarray__":"i5CP+rQhM0Ar8FGwtlIzQIW0KYddmyxA7Dx4HmVOOEArqHwOu0Y1QJkw7ZMt3DNAfaDrH2yVNEARoV8J2RI2QFBo1ow8PThAwyXPTmjOOUClPO4oMmgxQHZMS6xA2zRABySuczFrNkCFmCVPtlc5QH7RpAOXtzNAOT5FJGvIM0Ajvzilrlk1QPFS4cqe8DFASdluufz5OEDAC5+JRkk5QFoZEx7VMzVA6FaYHL7dMkAdwvH/6x8qQP89V6vDnTNAoFV7DhfxM0B+lS6aP8o2QNgBpbsedTNAx6bfxbxWNEC0IVk/IwI3QEvdkvxQPzRASVJY6VHeNUD6Np0XtDg3QF55yPwwKThA+FGkFh0WPEBKv41YhHY3QLpRDezS7DtAqTLPm3fLN0AXMBViqXc0QK4KUksf4TZAoGOAHNxnNUD4AZjoV0o3QFNahFIBSjRAd9KiwmtSNkCGboU2KCsyQCbmLvTzIi9ApvDD4PgFNkCqIsX93dozQO5vFbNnQjpA7gykMQlVM0CN6NpWw1UyQBDpBFmCJzlAHw48LBcWMkCxlou3yLo5QBHuz97M/DlA66dGO3atOEC6N66fZxkqQHuKDbJefzlAwDI0C1/hN0BCM2CENPw2QDI68VJ+AjpAEi8PX2WqNUBmzfHfBBE+QBrE1Bt+GjRAjLbjPt9cMUAxdnLQrFM2QANd7WmWiTFA7y8d2B4aN0DiKhDsXDY6QMrPmOHIAzhA","dtype":"float64","order":"little","shape":[69]}},"selected":{"id":"2826"},"selection_policy":{"id":"2843"}},"id":"2825","type":"ColumnDataSource"},{"attributes":{"zoom_on_axis":false},"id":"2780","type":"WheelZoomTool"},{"attributes":{"fill_color":{"field":"color","transform":{"id":"2824"}},"hatch_color":{"field":"color","transform":{"id":"2824"}},"line_color":{"field":"color","transform":{"id":"2824"}},"size":{"field":"size"},"x":{"field":"Lon"},"y":{"field":"Lat"}},"id":"2831","type":"Scatter"},{"attributes":{"match_aspect":true,"overlay":{"id":"2783"}},"id":"2781","type":"BoxZoomTool"}],"root_ids":["2748"]},"title":"Bokeh Application","version":"2.4.1"}};
    var render_items = [{"docid":"ede503f3-3623-4a9f-8f57-87d990f1b2f0","root_ids":["2748"],"roots":{"2748":"2563a670-c0a2-4c13-b8fe-b323b73374e7"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



### Step 5: Use the interactive map to answer the following question:

**Question:** Which neighborhood has the highest gross rent, and which has the highest sale price per square foot?

**Answer:** The neighborhood with the highest gross rent is Westwood Park, and the neighborhood highest sale price per square foot is Union Square District.

## Compose Your Data Story

Based on the visualizations that you have created, compose a data story that synthesizes your analysis by answering the following questions:

**Question:**  How does the trend in rental income growth compare to the trend in sales prices? Does this same trend hold true for all the neighborhoods across San Francisco?

**Answer:** The increase in rental income doesn't seem to have a lot to do with the price of sqr foot of property.  With the above question of Step 5 and according to the chart in the cell above, Merced Heights has a higher price per square foot, but a lower rental rent.  The inverse is true for Westwood Park.  Looking at the other neighborhoods, the gross rent cost and price per square foot seem to almost be seperate from one another.  There isn't a consistent trent, but what I've noticed though is the water front property price per square foot and the inner city properties price per square foot are higher than the stretch of land that lies in between.  Take for example Yerba Buena Price per square foot is $576, and Van Ness which lies more in the middle of the water front and inner city is $404, and Eureka Valley is $642.  Now this dip isn't consistent through out San Francisco, so it is most likely due to other variables as to why the price per square foot is so high, but it is interesting to me.  Another thing that is important to remember is the famous Golden Gate Bridge, the property Union Square District has the highest price per square foot, so that would mean easy access to the bridge for transportation is paramount.  

**Question:** What insights can you share with your company about the potential one-click, buy-and-rent strategy that they're pursuing? Do neighborhoods exist that you would suggest for investment, and why?

**Answer:** I would say to investors to think about what it is they are looking to buy, the use of it, and it's location.  If it's for commercial usage, meaning a lot of foot traffic like a shopping center or store, the best bet would the Yerba Buena Neighborhood, and The Presidio Neighborhood.  While the price per square foot is higher than some other places, it can be paid off in a quicker amount of time, not to mention the gross rent is lower there versus other places.  The base gross rent is what must be paid every month, so if you can invest in a place that has a lower gross rent, a great location, and a price per square foot that doesn't kill your wallet, then you're setting yourself up for success with a great location to invest in for commercial usage.  Keep in mind Yerba Buena does have a bit of a higher price per square foot, but that can be made up for with a higher volume of foot traffic in the area.


```python

```
