# Checkmk Host Matrix Snapin

In early 2022, as part of v2.1.0b1, the "Host Matrix" snapin was dropped from the official tribe29/checkmk repo as part of https://checkmk.com/werk/13736, via commit 
[0bd1c5a](https://github.com/tribe29/checkmk/commit/0bd1c5acc37c5676cc11f48ec760ebcf940a5773). I still wanted to use the snapin though, so have cloned and updated it to work with v2.1.0+.


## How to use
1. Copy [host_matrix.py](https://raw.githubusercontent.com/finalduty/checkmk-hostmatrix-snapin/main/host_matrix.py) to `~/local/lib/python3/cmk/gui/plugins/sidebar/`
2. Restart your omd instance
3. Done


## Changes to original
Where possible, I've kept the snapin as close to the latest version available in the 2.0.0 branch, however made some changes as below:
- Merged the contents of [~/lib/python3/cmk/gui/matrix_visualization.py](https://github.com/tribe29/checkmk/blob/2.0.0/cmk/gui/matrix_visualization.py) and [~/lib/python3/cmk/gui/plugins/sidebar/host_matrix.py](https://github.com/tribe29/checkmk/blob/2.0.0/cmk/gui/plugins/sidebar/host_matrix.py) in to one file. (These were originally in the same file, prior to [8ef8b00](https://github.com/tribe29/checkmk/commit/8ef8b0007876af61362eae88a1953c6a67799f00))
- Updated the 'from' for CustomizableSidebarSnapin, snapin_registry, snapin_width was moved from cmk.gui.plugins.sidebar to cmk.gui.plugins.sidebar.utils
- Change urlencode from html in cmk.gui.globals, to just import from cmk.gui.utils.urls
