# GAMIT-GLOBK
📋 Requirements
Software Requirements
Ubuntu Linux or similar Linux environment.
Python 3.8 or newer.
Bash shell.
GAMIT/GLOBK version 10.7 or newer.
Required Linux tools available in PATH:
tcsh
gcc
gfortran
make
curl
gzip
gunzip
awk
sed
python3

Before running the launcher, make sure the GAMIT/GLOBK environment is loaded:

source ~/.bashrc

Check that the required GAMIT/GLOBK commands are available:

which sh_gamit
which sh_glred
which globk
which glorg
🖥️ Tested Environment

This launcher was tested on:

Operating System : Ubuntu 20.04.6 LTS
Codename         : focal
Shell            : Bash
Python           : Python 3.8+
Platform         : Linux

The message below is normal and does not affect processing:

No LSB modules are available.
📡 Recommended RINEX Data Preparation

For stable GAMIT/GLOBK processing, prepare the RINEX files carefully before running the launcher.

1. Recommended Sampling Interval

Use:

30-second observation interval
24-hour daily observation file

A complete 24-hour RINEX file with 30-second sampling normally contains about:

2880 epochs per day

Avoid mixing different sampling intervals in the same project unless the files are first converted or decimated to a common interval.

2. Recommended RINEX Version

For GPS-only GAMIT processing, the safest and most compatible format is:

RINEX version : 2.11
Satellite     : GPS only
File type     : Observation file

Recommended short-name RINEX format:

ssssddd0.yyo

where:

ssss = 4-character station code
ddd  = day of year
0    = daily session
yy   = two-digit year
o    = observation file

Example:

hyd20910.26o

This means:

Station = hyd2
DOY     = 091
Year    = 2026
Type    = RINEX observation file

Compressed files are also accepted:

.26o.gz
.26o.Z
.26d
.26d.gz
.26d.Z
3. GPS-Only Processing

This launcher is mainly prepared for GPS-only processing.

Recommended GAMIT/GLOBK setting:

GNSS = G

If your receiver provides multi-GNSS data such as GPS, GLONASS, Galileo, or BeiDou, prepare or filter the RINEX file as GPS-only before final scientific processing.

4. RINEX 3 Data

RINEX 3 files may be available in long-name format, for example:

HYDE00IND_R_20260910000_01D_30S_MO.rnx.gz

The launcher has handling for some long-name and compressed RINEX files, but for the most stable GAMIT workflow, GPS-only short-name RINEX 2.11 files are recommended.

Recommended final format:

hyd20910.26o
hyd20920.26o
hyd20930.26o
5. Data Completeness

Before processing, check that every selected station has data for all selected days.

Example for 5 days starting from DOY 091:

Expected DOYs: 091 092 093 094 095

If one station is missing a day, the launcher will report:

MISSING_DAYS

If one station has fewer epochs than expected, the launcher may report:

PARTIAL

Partial stations should be checked carefully before final scientific processing.

6. Station Metadata

Before final processing, verify the following metadata:

station.info
receiver type
antenna type
antenna height
marker name
APPROX POSITION XYZ

Incorrect antenna height, wrong antenna type, or wrong receiver metadata can affect the final coordinate solution.

🧰 Installation

Clone the repository:

git clone https://github.com/yourusername/GAMIT-GLOBK-Launcher.git
cd GAMIT-GLOBK-Launcher

Make the script executable:

chmod +x GamiT_GlobK_Glorg_FINAL.py

Run the launcher:

./GamiT_GlobK_Glorg_FINAL.py

or:

python3 GamiT_GlobK_Glorg_FINAL.py
▶️ Usage

After running the script, the interactive menu provides three options:

1. Run GAMIT only
2. Run GLOBK/GLORG only
3. Run full GAMIT first, then GLOBK/GLORG dashboard
Typical Workflow
Provide the source RINEX directory.
Enter year, start DOY, number of days, experiment code, and project folder name.
Review the RINEX station summary.
Exclude bad or partial stations if needed.
Rename stations if required.
Select or confirm the APR/reference frame.
Run GAMIT processing.
Run GLOBK/GLORG processing.
Review final coordinate outputs.
Optionally generate epoch-propagated coordinates.
📁 Suggested RINEX Folder Arrangement

Keep all source RINEX files in one folder, for example:

/home/geodesy/Documents/cors-rinex

Example contents:

hyd20910.26o
hyd20920.26o
hyd20930.26o
hyd20940.26o
hyd20950.26o
indo0910.26o
indo0920.26o
indo0930.26o
indo0940.26o
indo0950.26o

Then provide this folder path when the launcher asks:

Path to local/source RINEX files:
📁 Outputs

The launcher creates a project folder such as:

~/cors2026/

Typical contents include:

rinex/
tables/
igs/
ionex/
brdc/
gsoln/
globk_glorg_logs/

Important final outputs include:

gsoln/final_outputs/local_final_coordinates.csv
gsoln/final_outputs/pos_statistics.txt
gsoln/final_outputs/local_coordinates_epoch_*.csv

The GLOBK/GLORG solution files are stored in:

gsoln/

Daily .org and combined .org files are saved there.

🌐 Downloads and Authentication

The launcher may download required products such as:

SP3 orbit files
BRDC navigation files
IONEX ionosphere files
Ocean tide loading grid

For CDDIS/Earthdata downloads, configure ~/.netrc:

machine urs.earthdata.nasa.gov
login YOUR_EARTHDATA_USERNAME
password YOUR_EARTHDATA_PASSWORD

Set safe file permission:

chmod 600 ~/.netrc
touch ~/.urs_cookies
chmod 600 ~/.urs_cookies
🧭 Epoch Propagation and Velocity

The script can optionally propagate final coordinates to a user-specified epoch.

Velocity sources may include:

Measured velocity from long-term GLOBK/POS time series.
External station velocity CSV.
India plate model velocity for demonstration.

Important note:

India plate model velocity is not official station-specific CORS velocity.

For final scientific or official coordinates, use measured station velocities derived from long-term GNSS processing or official station velocity values.

Short data spans such as 5 days are not sufficient for reliable station velocity estimation. For measured station velocity, use long-term data, preferably more than one year, and ideally 2–3 years or more.

⚙️ Customisation

The script can be customised by editing:

Default IGS/reference station list
APR_CANDIDATES_BY_FRAME
Default experiment settings
India plate velocity constants
External velocity CSV template

Users may also provide their own station velocity CSV for epoch propagation.

⚠️ Scientific Caution

This launcher automates the processing workflow, but the user must still verify the scientific quality of the results.

Before using coordinates in any official or publication-level work, check:

RINEX quality
station.info metadata
antenna height
receiver and antenna type
reference frame
APR file
earthquake file
ocean tide loading
orbit products
ionosphere products
GLOBK stabilization sites
velocity source for epoch propagation

The software assists processing, but final interpretation and validation remain the responsibility of the user.

📜 License

This project is licensed under the MIT License. See the LICENSE file for details.

🤝 Contributing

Contributions are welcome. Users may open an issue or submit a pull request for bug fixes, improvements, documentation updates, or new features.

🙏 Acknowledgements
MIT GAMIT/GLOBK team for the GAMIT/GLOBK software.
CDDIS and IGS for GNSS data and products.
Jade et al. (2017) for the India plate motion model used for demonstration-level epoch propagation.
https://drive.google.com/file/d/1S9jqvaQ5gPgECQc2dNUcOKoZpQlrM5T4/view?usp=sharing   //
use this GamiT_GlobK_Glorg_FINAL.py
