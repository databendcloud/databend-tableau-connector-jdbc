# databend-tableau-connector-jdbc
Tableau connector to Databend using JDBC driver

## Intro

This is an extension for Tableau Desktop / Tableau Server that simplifies the process of connecting Tableau to Databend and extends support for standard Tableau functionality when working with Databend (as compared to Generic ODBC/JDBC)

## Features

- In comparison with **Other Databases (ODBC)**: this connector uses the JDBC driver, which is faster than the ODBC driver in some cases (for example, creating Extracts), and is also much easier to install than ODBC (a cross-platform jar file, which does not require compiling for individual platforms).
- In comparison with **Other Databases (JDBC)**: this connector has fine-tuning SQL queries to implement most of the standard Tableau functionality (including multiple JOINS in the data source, Sets, etc.), and it has a friendly connection dialog ;)

## Before you install

Requirements
- Tableau **2022.3+**
- Databend **1.0.0+**

## Installation (Tableau Desktop)
1. Download the [Databend JDBC Driver](https://github.com/databendcloud/databend-jdbc) (version >= 0.3.4 required), and place the `databend-jdbc-0.3.4.jar` to:
    - macOS: `~/Library/Tableau/Drivers`
    - Windows: `C:\Program Files\Tableau\Drivers`
    - You need to create the folder if it doesn't already exist
2. Download the latest `databend_jdbc.taco` from the [Releases](https://github.com/databendcloud/databend-tableau-connector-jdbc/releases) page, and place it to:
    - macOS: `~/Documents/My Tableau Repository/Connectors`
    - Windows: `C:\Users\[Windows User]\Documents\My Tableau Repository\Connectors`
3. Run Tableau Desktop With Disabling signature verification
   - On Tableau Desktop, you use this command:  /Applications/Tableau\ Desktop\ 2022.3.app/Contents/MacOS/Tableau  -DDisableVerifyConnectorPluginSignature=true
   - On server, you can disable signature verification by setting native_api.disable_verify_connector_plugin_signature to true via TSM.
4. In Tableau Desktop: **Connect** ➔ **To a Server** ➔ **Databend JDBC by Databend, Inc.**

## Installation (Tableau Prep Builder)
1. Download the [Databend JDBC Driver](https://github.com/databendcloud/databend-jdbc) (version >=0.3.4 required) and place the `databend-jdbc-0.3.4.jar` to:
    - macOS: `~/Library/Tableau/Drivers`
    - Windows: `C:\Program Files\Tableau\Drivers`
    - You need to create the folder if it doesn't already exist
2. Download the latest `databend_jdbc.taco` from the [Releases](https://github.com/databendcloud/databend-tableau-connector-jdbc/releases) page and place it to:
    - macOS: `~/Documents/My Tableau Prep Repository/Connectors`
    - Windows: `C:\Users\[Windows User]\Documents\My Tableau Prep Repository\Connectors`
3. Run Tableau Prep Builder
4. In Tableau Prep Builder: **Connections** ➔ **+** ➔ **To a Server** ➔ **Databend JDBC by Databend, Inc.**

## Installation (Tableau Server)
1. Download the [Databend JDBC Driver](https://github.com/databendcloud/databend-jdbc) (version >=0.3.4 required) and place the `databend-jdbc-0.3.4.jar` to:
    - Linux: `/opt/tableau/tableau_driver/jdbc`
    - Windows: `C:\Program Files\Tableau\Drivers`
    - You need to create the directory if it doesn't already exist
    - *For Linux:* make sure directory is readable by the "tableau" user. To do this:
        - Create the directory:
            ```
            sudo mkdir -p /opt/tableau/tableau_driver/jdbc
            ```
        - Copy the downloaded driver file to the location, replacing `[/path/to/file]` with the path and `[driver file name]` with the name of the driver you downloaded:
            ```
            sudo cp [/path/to/file/][driver file name].jar /opt/tableau/tableau_driver/jdbc
            ```
        - Set permissions so the file is readable by the "tableau" user, replacing `[driver file name]` with the name of the driver you downloaded:
            ```
            sudo chmod 755 /opt/tableau/tableau_driver/jdbc/[driver file name].jar
            ```
2. Download the latest `databend_jdbc.taco` from the [Releases](https://github.com/databendcloud/databend-tableau-connector-jdbc/releases) page and place it into these folders on each node:
    - Linux: `/opt/tableau/connectors`
    - Windows: `C:\Program Files\Tableau\Connectors`
3. Restart the server.
    ```
    tsm restart
    ```
    - Note that whenever you add, remove, or update a connector, you need to restart the server to see the changes.

4. Disable sign validation when starting Tableau app

```
./Tableau -DDisableVerifyConnectorPluginSignature=true
```


## Connection tips
### Initial SQL tab
Feel free to set session level [settings](https://databend.rs/doc/sql-reference/system-tables/system-settings) using
```
SET my_setting=value;
```
### Advanced tab
In 99% of cases you don't need the Advanced tab, for the remaining 1% you can use the following settings:
- **Custom Connection Parameters**. By default, `query_timeout` is already specified, this parameter may need to be changed if some extracts are updated for a very long time. The value of this parameter is specified in milliseconds. The rest of the parameters can be found [here](https://github.com/databendcloud/databend-jdbc/blob/main/docs/Connection.md), add them in this field separated by commas

- **JDBC Driver URL Parameters**. You can pass the remaining [driver parameters](https://github.com/databendcloud/databend-jdbc/blob/main/docs/Connection.md), for example `qyery_timeout`, in this field. Be careful, the parameter values must be passed in the URL Encoded format, and in the case of passing `custom_http_params` or `typeMappings` in this field and in the previous fields of the Advanced tab, the values of the preceding two fields on the Advanced tab have a higher priority

## Future plans
- Publishing the connector at [exchange.tableau.com](https://exchange.tableau.com/connectors)
