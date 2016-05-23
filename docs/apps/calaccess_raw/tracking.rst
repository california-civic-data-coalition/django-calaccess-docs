Models for tracking updates
===========================

The raw-data app also keeps track of each version of the CAL-ACCESS database released by the California Secretary of State, including its release date and byte size, as well as the activity of the `management commands <http://django-calaccess-raw-data.californiacivicdata.org/en/latest/managementcommands.html>`_ that process this data.

This tracking information is stored in the data tables outlined below.

.. note::

    By default, the raw-data app does *not* archive previous versions of the CAL-ACCESS database. Rather, with each call to the management commands, the data files they process are overwritten by new versions.

    You can configure the raw-data app to keep each version of the zip file downloaded from the California Secretary of State as well as the indivdual raw .csv files and cleaned .tsv files by flipping the ``CALACCESS_STORE_ARCHIVE`` to ``True`` in ``settings.py``:

    .. code-block:: python

        # in settings.py
        CALACCESS_STORE_ARCHIVE = True

    The older versions of these files will then be saved to the path specified by your Django project's ``MEDIA_ROOT`` setting (more on that `here <https://docs.djangoproject.com/en/1.9/ref/settings/#media-root>`_).

----------------------

RawDataVersion
~~~~~~~~~~~~~~

Versions of CAL-ACCESS raw source data, typically released every day.

**Fields:**

.. raw:: html

    <div class="wy-table-responsive">
    <table border="1" class="docutils">
    <thead valign="bottom">
        <tr>
            <th class="head">Name</th>
            <th class="head">Type</th>
            <th class="head">Unique key</th>
            <th class="head">Definition</th>
        </tr>
    </thead>
    <tbody valign="top">




        <tr>
            <td>id</td>
            <td>Integer</td>
            <td>Yes</td>
            <td>Auto-incrementing unique identifer of versions</td>
        </tr>



        <tr>
            <td>release_datetime</td>
            <td>DateTime</td>
            <td>No</td>
            <td>(Unique) date and time the version of the CAL-ACCESS database was released (value of Last-Modified field in HTTP response header)</td>
        </tr>



        <tr>
            <td>Size</td>
            <td>Integer</td>
            <td>No</td>
            <td>Size of the .ZIP file for this version of the CAL-ACCESS raw source data (value of content-length field in HTTP response header)</td>
        </tr>



        <tr>
            <td>zip_file_archive</td>
            <td>FileField</td>
            <td>No</td>
            <td>An archive of the original zipped file downloaded from CAL-ACCESS.</td>
        </tr>
    </tbody>
    </table>
    </div>

----------------------

RawDataFile
~~~~~~~~~~~

Data files included in the given version of the CAL-ACCESS raw source data.

**Fields:**

.. raw:: html

    <div class="wy-table-responsive">
    <table border="1" class="docutils">
    <thead valign="bottom">
        <tr>
            <th class="head">Name</th>
            <th class="head">Type</th>
            <th class="head">Unique key</th>
            <th class="head">Definition</th>
        </tr>
    </thead>
    <tbody valign="top">




        <tr>
            <td>id</td>
            <td>Integer</td>
            <td>Yes</td>
            <td>Auto-incrementing unique identifer of the file</td>
        </tr>



        <tr>
            <td>version_id</td>
            <td>Integer</td>
            <td>No</td>
            <td>Foreign key referencing the version of the raw source data in which the file was included</td>
        </tr>



        <tr>
            <td>file_name</td>
            <td>String (up to 100)</td>
            <td>No</td>
            <td>Name of the raw source data file without extension</td>
        </tr>



        <tr>
            <td>download_records_count</td>
            <td>Integer</td>
            <td>No</td>
            <td>Count of records in the original file downloaded from CAL-ACCESS</td>
        </tr>



        <tr>
            <td>clean_records_count</td>
            <td>Integer</td>
            <td>No</td>
            <td>Count of records in the cleaned file generated by calaccess_raw</td>
        </tr>



        <tr>
            <td>load_records_count</td>
            <td>Integer</td>
            <td>No</td>
            <td>Count of records in the loaded from cleaned file into calaccess_raw's data model</td>
        </tr>



        <tr>
            <td>download_column_count</td>
            <td>Integer</td>
            <td>No</td>
            <td>Count of columns in the original file downloaded from CAL-ACCESS</td>
        </tr>



        <tr>
            <td>clean_column_count</td>
            <td>Integer</td>
            <td>No</td>
        	<td>Count of columns in the cleaned file generated by calaccess_raw</td>
        </tr>



        <tr>
            <td>load_column_count</td>
            <td>Integer</td>
            <td>No</td>
            <td>Count of columns on the loaded calaccess_raw data model</td>
        </tr>



        <tr>
            <td>clean_file_archive</td>
            <td>FileField</td>
            <td>No</td>
            <td>An archive of the original raw data file downloaded from CAL-ACCESS.</td>
        </tr>



        <tr>
            <td>zip_file_archive</td>
            <td>FileField</td>
            <td>No</td>
            <td>An archive of the raw data file after being cleaned.</td>
        </tr>
   	</tbody>
    </table>
    </div>

----------------------

RawDataCommand
~~~~~~~~~~~~~~

Start and finish times for calls to CAL-ACCESS related management commands

**Fields:**

.. raw:: html

    <div class="wy-table-responsive">
    <table border="1" class="docutils">
    <thead valign="bottom">
        <tr>
            <th class="head">Name</th>
            <th class="head">Type</th>
            <th class="head">Unique key</th>
            <th class="head">Definition</th>
        </tr>
    </thead>
    <tbody valign="top">




        <tr>
            <td>id</td>
            <td>Integer</td>
            <td>Yes</td>
            <td>Auto-incrementing unique identifer of the command log</td>
        </tr>



        <tr>
            <td>version_id</td>
            <td>Integer</td>
            <td>No</td>
            <td>Foreign key referencing the version of the raw source data that was being acted on</td>
        </tr>



        <tr>
            <td>command</td>
            <td>String (up to 50)</td>
            <td>No</td>
            <td>Name of the command performed on the given version of the raw source data</td>
        </tr>



        <tr>
            <td>called_by</td>
            <td>Integer</td>
            <td>No</td>
            <td>Foreign key refencing log of the CalAccessCommand that called this command.Null represents call from command line</td>
        </tr>



        <tr>
            <td>file_name</td>
            <td>String (up to 100)</td>
            <td>No</td>
            <td>Name of the raw source data file without extension</td>
        </tr>



        <tr>
            <td>start_datetime</td>
            <td>DateTime</td>
            <td>No</td>
            <td>Date and time when the given command started on the given version of the raw source data</td>
        </tr>



        <tr>
            <td>finish_datetime</td>
            <td>DateTime</td>
            <td>No</td>
            <td>Date and time when the given command finished on the given version of the raw source data</td>
        </tr>

    </tbody>
    </table>
    </div>