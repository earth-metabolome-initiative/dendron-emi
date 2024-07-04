---
id: v5zls2vytueekq9twor2qck
title: '2024-07-03'
desc: ''
updated: 1720012918829
created: 1720008914994
traitIds:
  - open-notebook-emi-mjopiti
---

# This is Michael's EMI open-notebook.

Today is 2024.07.03

## Todo today

### Have a look at the EMI discussion forum
    - https://github.com/orgs/earth-metabolome-initiative/discussions
###
###

## Doing
Exploring XML files to automatize mzmine batch mode


## File structure
Header: 
      - <?xml version="1.0" encoding="UTF-8"?><batch mzmine_version="4.1.0">

Several batchsteps:
      - <batchstep method="io.github.mzmine.modules.io.import_rawdata_bruker_tdf.TDFImportModule" parameter_version="1">
          <parameter name="File names">
              <file>/Path/to/directory/for/files</file>
          </parameter>
        </batchstep>
? what do the batchsteps represent in the GUI? How can they be shortcutted for CLI?

## Done

## Notes
Mzmine uses academic log-in to grant the use of the application. Here's the link to [the discussion on github](https://github.com/mzmine/mzmine/issues/1827)
-> Summary: It is possible to define the user in the config file but we will soon push a new release that takes a user file or credentials as input.
  -> <preferences>
        ....
        <parameter name="username">robinschmid</parameter> 
     </preferences>

[Here](https://mzmine.github.io/mzmine_documentation/services/users.html) the reference to the documentation to implement an user.
And here the help from mzmine app:
  -l,--libraries <arg>    spectral library files. Either defined in a .txt
                        text file with one file per line
                        or by glob pattern matching. To match all .json
                        or .mgf files in a path: -l "D:\Data\*.json
  --list-users         List all users that are available and logged in
                        on this system (computer or server).
  --login              Use command-line to login to a user. This will
                        open the login website in the system internet
                        browser, if supported,
                        or prompts an input for the user credentials into
                        the console. After successful login,
                        a user file will be copied to your system USER
                        directory in /.mzmine/users/. And the current
                        user will be saved to the configuration.
                        The created user file can be accessed with the
                        -user argument on the next startup.


Where to change the directory/file names? 
  -> beginning for file name
  ->     <batchstep method="io.github.mzmine.modules.io.import_rawdata_bruker_tdf.TDFImportModule" parameter_version="1">
            <parameter name="File names">
                <file>/home/jopitim/test/cuso_batch_00001</file>
            </parameter>
        </batchstep>
      (Line ~100)
  -> <batchstep method="io.github.mzmine.modules.dataanalysis.feat_ms2_similarity_intra.IntraFeatureRowMs2SimilarityModule" parameter_version="1">
        <parameter name="Feature lists" type="BATCH_LAST_FEATURELISTS"/>
        <parameter name="Export to file" selected="false">
            <current_file>/home/jopitim/test/mapp_batch_00106/Untitled_intra_row_MS2similarity</current_file>
        </parameter>
        <parameter name="m/z tolerance (MS2)">
            <absolutetolerance>0.003</absolutetolerance>
            <ppmtolerance>10.0</ppmtolerance>
        </parameter>
        <parameter name="Minimum signals">4</parameter>
        <parameter name="Signal filters">
            <parameter name="Remove residual precursor m/z" selected="true">10.0</parameter>
            <parameter name="Crop to top N signals">250</parameter>
            <parameter name="Signal threshold (intensity filter)">50</parameter>
            <parameter name="Intensity filter at &gt;N signals">0.98</parameter>
        </parameter>
    </batchstep>
    (line ~500)

        
## Todo tomorrow, one day ... or never

###
###
###


## Today I learned that

-