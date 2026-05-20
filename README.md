# Checkmk Host Matrix Snapin

In early 2022, as part of v2.1.0b1, the "Host Matrix" snapin was dropped from the official tribe29/checkmk repo as part of https://checkmk.com/werk/13736, via commit 
[0bd1c5a](https://github.com/tribe29/checkmk/commit/0bd1c5acc37c5676cc11f48ec760ebcf940a5773). I still wanted to use the snapin though, so have cloned and updated it to work with v2.1.x and v2.2.x.


## How to use
1. Log in as the CMK user: `sudo -iu <site_user>`
2. Make a local sidebar plugin directory if it doesn't exist: `mkdir -pv ~/local/lib/python3/cmk/gui/plugins/sidebar`
3. Copy the appropriate host_matrix.py verion for your install to the plugin directory.
    - For CheckMK 2.1.x: [host_matrix_2.1.x.py](https://raw.githubusercontent.com/finalduty/checkmk-hostmatrix-snapin/main/host_matrix_2.1.x.py)
    - For CheckMK 2.2.x: [host_matrix_2.2.x.py](https://raw.githubusercontent.com/finalduty/checkmk-hostmatrix-snapin/main/host_matrix_2.2.x.py) 
4. Restart your omd instance: `omd restart`
5. Done


## Changes to original
Where possible, I've kept the snapin as close to the latest version available in the 2.0.0 branch, however made some changes as below:

### v2.5.x
- Update HostMatrixSnapin.show() signature to match parent class

### v2.4.x
- Remediate CustomizableSidebarSnapin imports, which moved at some point after 2.3
- Update get_filter_headers parameters 

### v2.2.x
- Import 'html' from cmk.gui.htmllib.html as cmk.gui.globals no longer exists

### v2.1.x
- Merged the contents of [~/lib/python3/cmk/gui/matrix_visualization.py](https://github.com/tribe29/checkmk/blob/2.0.0/cmk/gui/matrix_visualization.py) and [~/lib/python3/cmk/gui/plugins/sidebar/host_matrix.py](https://github.com/tribe29/checkmk/blob/2.0.0/cmk/gui/plugins/sidebar/host_matrix.py) in to one file. (These were originally in the same file, prior to [8ef8b00](https://github.com/tribe29/checkmk/commit/8ef8b0007876af61362eae88a1953c6a67799f00))
- Updated the 'from' for CustomizableSidebarSnapin, snapin_registry, snapin_width was moved from cmk.gui.plugins.sidebar to cmk.gui.plugins.sidebar.utils
- Change urlencode from html in cmk.gui.globals, to just import from cmk.gui.utils.urls

## CSS issues
As this is an unsupported plugin, CSS support will eventually be removed from the built-in themes. As of about v2.4.x, the following custom CSS is required to restore the blue color for hosts in downtime (i.e. "stated", where 'd' = downtime):

    OMD[your_site]:~$ cat << EOF > ~/local/share/check_mk/web/htdocs/css/hostmatrix.css
    > .hostmatrix .stated {
        background-color: #0af !important;
        border: 1px solid #0af !important;
        color: #000 !important;
    }
    EOF

Note: this CSS override mechanism is deprecated. The "correct" method is to duplicate and customize the built-in theme, but this may be overkill depending on your requirements.

## Development / Contributing
1. Install the desired Raw edition from the CMK website - https://checkmk.com/download?edition=cre
2. Copy the CMK GUI code from /opt/omd/versions/*/lib/python3/cmk/ to your local development environment
3. Save a new version of host_matrix.py with a suffix suitable to the version being developed against
