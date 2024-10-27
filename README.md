# 5200group2

## Data

- **chicago_shapefile/**: Contains shapefiles for Chicago census tracts.  ([download link](https://data.cityofchicago.org/api/geospatial/5jrd-6zik?method=export&format=Shapefile))
- **la_shapefile/**: Contains shapefiles for Los Angeles census tracts. ([download link](https://geohub.lacity.org/datasets/la-city-2020-census-tracts-/explore))
- **clean crime python/**: Analyzes crime data for Chicago and Los Angeles.

## Usage

1. **Install Dependencies**:
    ```sh
    pip install pandas geopandas
    ```

2. **Run Notebooks**:
    Open and run the files in clean crime python/ (`crime_chicago.ipynb`, `crime_la.ipynb`) to perform the analysis.

## Functions

### aggregate_points

Aggregates crime points by census tract and type of crime.

```python
def aggregate_points(points_gdf, geometry_gdf):
    points_gdf = gpd.sjoin(points_gdf, geometry_gdf, how='inner', predicate='within')
    points_gdf = points_gdf.groupby(['geoid10', 'Primary Type']).size().reset_index(name='count')
    return points_gdf
```
## Results

The results are saved as CSV files in Jupyter notebooks.

- `crime_chicago_2001_2024_by_tract_type.csv`
- `crime_la_2020_2024_by_tract_type.csv`

## License

This project is licensed under the MIT License.
