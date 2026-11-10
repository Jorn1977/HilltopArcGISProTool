# Import Hilltop Collection Sites

[<video src="./Hilltop_Import_Tool_LinkedIn.mp4" controls width="720"></video>](https://github.com/Jorn1977/HilltopArcGISProTool/blob/main/hilltop_thumbnail_modern.png)

### Description

Downloads a Hilltop monitoring collection from a New Zealand regional council (Horizons, GWRC, TRC, NRC or Southland) and creates a WGS 1984 point feature class of the selected sites, with an optional recordings table and CSV export.

### Usage

- Start by choosing a Council. The active map zooms to that council's region and the Collection list is loaded from the council's Hilltop CollectionList service.

- When a Collection is chosen, a feature class containing all locatable sites in the collection is created in the Output Geodatabase (default name CollectionSites), added to the map, symbolized with Circle 3 and labelled with SiteName.

- Sites are found by merging CollectionList (which sites/measurements belong to the collection) with SiteList (where the sites are). Sites without a location in SiteList are not shown; the dialog reports how many were omitted.

- Optionally draw a Map Extent (WGS 1984) to restrict the Measurement and Sites lists to sites inside it. The filter only affects the dialog lists.

- Optionally choose a Measurement to narrow the Sites list to sites that have that measurement. The same measurement filters the recordings and time series.

- Add one row per site in the Sites parameter. Selected sites are highlighted in the CollectionSites layer as you pick them.

- Include Last Recording writes the latest value, units and time per site/measurement to the recordings table. It is switched off automatically when the time series is saved to the table, because the series already includes the latest recording.

- Download Full Time Series uses the Hilltop GetData From/To parameters (day/month/year, date only). Ranges over 90 days show a warning and ranges over 365 days can take a long time. Choose to save the series to the table, export it to CSV, or both.

- The CSV file name is generated automatically if left blank; existing files are never overwritten (a numeric suffix is added). The folder defaults to the toolbox folder.

- All outputs (feature class, table, any new map) use WGS 1984 (WKID 4326). Existing outputs with the same names in the geodatabase are deleted and recreated.

- The Collection Feature Class and Feature Class Name must be different.

- Output fields - feature class: SiteName, Longitude, Latitude, CollectionName, Council. Recordings table: SiteName, MeasurementName, Units, MeasurementTime (Date), MeasurementValue (Double), CollectionName, Council.

- Diagnostic logs are written next to the toolbox (HilltopToolbox_debug.log and HilltopCollectionSites_*.log).

- An internet connection to the selected council's Hilltop server is required.

### Syntax

ImportHilltopCollectionSites (council, collection, collection_feature_class, {extent_filter}, {measurement}, sites, {include_last_recording}, {map_name}, map_destination, {existing_map}, {download_timeseries}, {from_date}, {to_date}, {add_timeseries_to_feature_class}, {export_timeseries_csv}, {csv_file_name}, {csv_output_folder}, output_gdb, feature_class_name)

| Parameter                                    | Explanation                                                                                                                                                                                                                                                                           | Data Type   |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `council`                                    | The regional council whose Hilltop server is used: Manawatu-Whanganui (Horizons), Greater Wellington (GWRC), Taranaki (TRC), Northland (NRC) or Southland. All collection, site and data requests are scoped to this council. Selecting a council zooms the active map to its region. | String      |
| `collection`                                 | The Hilltop collection to import, listed from the council's CollectionList service. Selecting a collection adds all of its locatable sites to the active map.                                                                                                                         | String      |
| `collection_feature_class`                   | Name of the new feature class (default CollectionSites) holding every locatable site in the selected collection. It is created in the Output Geodatabase as soon as a council and collection are chosen. Must differ from the Feature Class Name.                                     | String      |
| `extent_filter (Optional)`                   | An optional extent (drawn on the map, converted to WGS 1984) used to restrict the Measurement and Sites lists to sites inside it. It affects only the dialog lists.                                                                                                                   | Extent      |
| `measurement (Optional)`                     | An optional measurement from the collection (for example 'Rainfall \[SCADA Rainfall\]'). Narrows the Sites list to sites with that measurement and filters the recordings and time series to the matching data source. Leave empty to include all measurements.                       | String      |
| `sites`                                      | One or more sites to include in the output feature class. Add a row per site and choose from the list. Sites are also selected on the map in the CollectionSites layer as they are picked.                                                                                            | Value Table |
| `include_last_recording (Optional)`          | Adds the latest recording (units, time, value) for each selected site/measurement to the recordings table. Checked - the last recording is included. (Default) Unchecked - it is not included. Disabled automatically when Save Time Series To Table is checked.                      | Boolean     |
| `map_name (Optional)`                        | Name of the new map when Map Destination is Create new map. Defaults to 'Hilltop Sites'.                                                                                                                                                                                              | String      |
| `map_destination`                            | Where the outputs are added. Add to existing/open map - uses the map chosen in Existing Map, or the active map. (Default) Create new map - creates a new WGS 1984 map.                                                                                                                | String      |
| `existing_map (Optional)`                    | The open map to add the outputs to. If blank, the active map is used.                                                                                                                                                                                                                 | String      |
| `download_timeseries (Optional)`             | Downloads every recording between the From and To dates using the Hilltop GetData service. Checked - enables the date, table and CSV options. Unchecked - no time series is downloaded. (Default)                                                                                     | Boolean     |
| `from_date (Optional)`                       | Start date of the time series. Date only; any time is ignored.                                                                                                                                                                                                                        | Date        |
| `to_date (Optional)`                         | End date of the time series. Date only; any time is ignored. Must be on or after the From Date. Large ranges take longer to download.                                                                                                                                                 | Date        |
| `add_timeseries_to_feature_class (Optional)` | Writes every downloaded recording to the \\_Recordings table, linked to the feature class by SiteName. Checked - the time series is saved to the table. Unchecked - it is not. (Default)                                                                         | Boolean     |
| `export_timeseries_csv (Optional)`           | Exports the downloaded time series to a CSV file with the columns SiteName, Measurement, Units, DateTime, Value and Council. At least one of the table or CSV options must be selected when downloading a time series. (Default: checked)                                             | Boolean     |
| `csv_file_name (Optional)`                   | Name of the CSV file. The .csv extension is added automatically. If blank, a name is generated from the council, collection and run time. Existing files are not overwritten.                                                                                                         | String      |
| `csv_output_folder (Optional)`               | Folder in which the CSV is written. Defaults to the toolbox folder.                                                                                                                                                                                                                   | Folder      |
| `output_gdb`                                 | The geodatabase that receives the collection feature class, the output feature class and the recordings table. Defaults to the project's default geodatabase.                                                                                                                         | Workspace   |
| `feature_class_name`                         | Name of the output sites-only point feature class (WGS 1984). The recordings table is named \\_Recordings. An existing dataset with the same name is replaced.                                                                                                   | String      |

### Code Samples

**ImportHilltopCollectionSites example (Python window)**

Imports two sites from a Southland collection, saves the last recording to a table and adds the results to a new map. The collection and site names are placeholders - use values from the tool dialog.

```python
import arcpy

arcpy.ImportToolbox(r"C:\GIS\Hilltop\HilltopToolbox.pyt")

arcpy.hilltoptools.ImportHilltopCollectionSites(
    council="Southland Region",
    collection="Rainfall",
    collection_feature_class="CollectionSites",
    extent_filter=None,
    measurement=None,
    sites="'Site A';'Site B'",
    include_last_recording=True,
    map_name="Hilltop Sites",
    map_destination="Create new map",
    existing_map=None,
    download_timeseries=False,
    from_date=None,
    to_date=None,
    add_timeseries_to_feature_class=False,
    export_timeseries_csv=False,
    csv_file_name=None,
    csv_output_folder=None,
    output_gdb=r"C:\GIS\Hilltop\Hilltop.gdb",
    feature_class_name="HilltopSites",
)
```

### Tags

Hilltop, Hydrology, Environmental monitoring, Time series, Rainfall, River flow, Water level, Water quality, Regional council, New Zealand, Horizons, Manawatu-Whanganui, Greater Wellington, GWRC, Taranaki, TRC, Northland, NRC, Southland, Environment Southland, Monitoring sites, CollectionList, SiteList, GetData, WGS 1984, Import

### Credits

Jorn Sijbertsma

### Use limitations

This toolbox is provided "as is", without warranty of any kind. The author accepts no liability for errors, omissions, or any loss or damage arising from its use.

The tools in this toolbox retrieve data from Hilltop Servers operated by individual councils. The data remains the property of the respective council and is subject to that council's own terms of use, licence, and attribution requirements. Users are responsible for checking and complying with those terms before using, sharing, or publishing any data obtained with this toolbox.

Data is provided as published by the council at the time of download. It may be provisional, unverified, or subject to revision, and may contain gaps or errors. Users should check data quality information and consult the council where accuracy matters.

This toolbox and the data it retrieves must not be relied on for safety-critical purposes, such as flood warnings, emergency response, or other real-time decisions. Use the official sources provided by the council for those purposes.

The tools depend on the availability and structure of council servers, which can change without notice and may stop the tools from working. Please keep downloads to a reasonable size, because requests for many sites or long date ranges place load on council servers, and servers may limit or block excessive use.

When using data obtained with this toolbox, acknowledge the council as the data source.

---

## Item Metadata

### Topics and Keywords

**Themes or categories of the resource:** Environment, Inland Waters, Location

### Citation

**Title:** Import Hilltop Collection Sites

**Alternate titles:** Hilltop data tool

**Collection title:** Hilltop data tool

### Resource Details

**Dataset languages:** English

**Dataset character set:** utf8 - 8 bit UCS Transfer Format

**Credits:**

Jorn Sijbertsma

**ArcGIS item properties** 

### Extents

**Extent** 

**Description:**

- Min longitude (west): 166.4
- Min latitude (south): -47.4
- Max longitude (east): 178.6
- Max latitude (north): -34.3

**Geographic extent** 

**Bounding rectangle** 

**Extent type** 

Extent used for searching

### Resource Constraints

**Constraints** 

**Limitations of use:**

This toolbox is provided "as is", without warranty of any kind. The author accepts no liability for errors, omissions, or any loss or damage arising from its use.

The tools in this toolbox retrieve data from Hilltop Servers operated by individual councils. The data remains the property of the respective council and is subject to that council's own terms of use, licence, and attribution requirements. Users are responsible for checking and complying with those terms before using, sharing, or publishing any data obtained with this toolbox.

Data is provided as published by the council at the time of download. It may be provisional, unverified, or subject to revision, and may contain gaps or errors. Users should check data quality information and consult the council where accuracy matters.

This toolbox and the data it retrieves must not be relied on for safety-critical purposes, such as flood warnings, emergency response, or other real-time decisions. Use the official sources provided by the council for those purposes.

The tools depend on the availability and structure of council servers, which can change without notice and may stop the tools from working. Please keep downloads to a reasonable size, because requests for many sites or long date ranges place load on council servers, and servers may limit or block excessive use.

When using data obtained with this toolbox, acknowledge the council as the data source.

### Distribution

Distributor

**Available format** 

**Name:** ArcToolbox Tool

### Metadata Details

**Metadata language:** English

**Metadata character set:** utf8 - 8 bit UCS Transfer Format

**Metadata identifier:** B72A2105-C895-4F61-8AF5-EB448475054A

**Scope of the data described by the metadata:** dataset

**Last update:** ⇔ 2026-11-07

**ArcGIS metadata properties**

**Metadata format:** ArcGIS 1.0

**Standard or profile used to edit metadata:** ISO19139

**Created in ArcGIS for the item:** 2026-10-29 22:51:43

**Last modified in ArcGIS for the item:** 2026-11-10 20:06:24

**Automatic updates**

**Have been performed:** No

### Metadata Contacts

**Metadata contact - owner** 

**Individual's name:** Jorn Sijbertsma

**Organization's name:** -

**Contact's position:** -

### Thumbnail and Enclosures

**Thumbnail** 

**Thumbnail type** 

Tool illustration

**Enclosure** 

**Enclosure type:**

JPG Side-panel Help Illustration

**Original file name:** thumbnail.jpg
