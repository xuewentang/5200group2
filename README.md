# 5200group2

## Data

- **chicago_shapefile/**: Contains shapefiles for Chicago census tracts.  ([download link](https://data.cityofchicago.org/api/geospatial/5jrd-6zik?method=export&format=Shapefile))
- **la_shapefile/**: Contains shapefiles for Los Angeles census tracts. ([download link](https://geohub.lacity.org/datasets/la-city-2020-census-tracts-/explore))
- **clean crime python/**: Analyzes crime data for Chicago and Los Angeles.
- **map_data/** prepare map data of chicago and Los Angeles for visualization in ARCGIS

## Usage

1. **Install Dependencies**:
    ```sh
    pip install pandas geopandas zipfile
    ```

2. **Run Notebooks**:
    Open and run the files in in the `clean crime python/` directory (`crime_chicago.ipynb`, `crime_la.ipynb`) to perform the analysis.

## Functions

### aggregate_points

Aggregates crime points by census tract and type of crime.

```python
def aggregate_points(points_gdf, geometry_gdf):
    points_gdf = gpd.sjoin(points_gdf, geometry_gdf, how='inner', predicate='within')
    points_gdf = points_gdf.groupby(['geoid10', 'Primary Type']).size().reset_index(name='count')
    return points_gdf
```
### zip map data

combine and create shapefile by year and geoid and zip them.

```python
combined_results_2020_2024 = combined_results[combined_results['Year'] >= 2020]
combined_results_2020_2024 = combined_results_2020_2024.groupby(['geoid10', 'Year']).sum().reset_index()
combined_results_2020_2024 = combined_results_2020_2024.drop(columns='Primary Type')
combined_results_2020_2024 = combined_results_2020_2024.merge(tracts_gdf, on='geoid10', how='left')
combined_results_2020_2024 = gpd.GeoDataFrame(combined_results_2020_2024, geometry='geometry')
combined_results_2020_2024['count'] = combined_results_2020_2024['count'].astype(int)
combined_results_2020_2024.to_file('map_data/crime_chicago_aggregated.shp')
# Zip the shapefile
with zipfile.ZipFile('map_data/crime_chicago_aggregated.zip', 'w') as z:
    z.write('map_data/crime_chicago_aggregated.shp')
    z.write('map_data/crime_chicago_aggregated.shx')
    z.write('map_data/crime_chicago_aggregated.dbf')
    z.write('map_data/crime_chicago_aggregated.prj')
    z.write('map_data/crime_chicago_aggregated.cpg')
```
## Results

The results are saved as CSV files in Jupyter notebooks.

- `crime_chicago_2001_2024_by_tract_type.csv`
- `crime_la_2001_2024_by_tract_type.csv`
- `crime_chicago_aggregated.zip`

## License

This project is licensed under the MIT License.
