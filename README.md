# beavis

Pandas and Dask test helper methods.

![cornholio](https://github.com/MrPowers/beavis/blob/main/images/cornholio.jpg)

## test helpers

### DataFrame comparisons

The built-in `pandas.testing.assert_frame_equal` method doesn't output an error message that's easy to parse, see this example.

```python
df1 = pd.DataFrame({'col1': [1, 2], 'col2': [3, 4]})
df2 = pd.DataFrame({'col1': [5, 2], 'col2': [3, 4]})
pd.testing.assert_frame_equal(df1, df2)
```

```
E   AssertionError: DataFrame.iloc[:, 0] (column name="col1") are different
E
E   DataFrame.iloc[:, 0] (column name="col1") values are different (50.0 %)
E   [index]: [0, 1]
E   [left]:  [1, 2]
E   [right]: [5, 2]
```

beavis provides a nicer error message.

```python
import beavis

beavis.assert_pd_equality(df1, df2)
```

![BeavisDataFramesNotEqualError](https://github.com/MrPowers/beavis/blob/main/images/beavis_dataframes_not_equal_error.png)

DataFrame comparison options:

* `check_index` (default `True`)
* `check_dtype` (default `True`)

Let's convert the Pandas DataFrames to Dask DataFrames and use the `assert_dd_equality` function to check they're equal.

```python
ddf1 = dd.from_pandas(df1, npartitions=2)
ddf2 = dd.from_pandas(df2, npartitions=2)
beavis.assert_dd_equality(ddf1, ddf2)
```

These DataFrames aren't equal, so we'll get a good error message that's easy to debug.

![Dask DataFrames not equal](https://github.com/MrPowers/beavis/blob/main/images/dd_not_equal.png)

### Column comparisons

Here's the built-in error message when comparing series that are not equal.

```python
df = pd.DataFrame({'col1': [1, 2, 9, 6], 'col2': [5, 2, 7, 6]})
pd.testing.assert_series_equal(df["col1"], df["col2"])
```

```
>   ???
E   AssertionError: Series are different
E
E   Series values are different (50.0 %)
E   [index]: [0, 1, 2, 3]
E   [left]:  [1, 2, 9, 6]
E   [right]: [5, 2, 7, 6]
```

Here's the beavis error message that's easier to read.

```python
beavis.assert_pd_column_equality(df, "col1", "col2")
```

![BeavisColumnsNotEqualError](https://github.com/MrPowers/beavis/blob/main/images/beavis_columns_not_equal_error.png)

