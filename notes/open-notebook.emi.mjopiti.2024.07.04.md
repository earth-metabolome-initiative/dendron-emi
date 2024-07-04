---
id: w1apexds30hsjxfdn3fqb08
title: '2024-07-04'
desc: ''
updated: 1720107838217
created: 1720105488247
traitIds:
  - open-notebook-emi-mjopiti
---

# This is Michael's EMI open-notebook.

Today is 2024.07.04

## Todo today

### Have a look at the EMI discussion forum
    - https://github.com/orgs/earth-metabolome-initiative/discussions
###
###

## Doing
Exploring XML files to automatize mzmine batch mode - Follow up

File used: /home/jopitim/mzmine-batch-mode-sandbox/docs/dbgi_project_00002/cuso_batch_00001/results/mzmine/cuso_batch_00001_batch.mzbatch

The file is a nested structure, where the root is batch (possibility to extend to multiple batches?).
The problem is that nested only for: batch -> batchstep -> parameter/files/modules
Already on the second step the tree structure starts to fall apart and case per case should be studied.


## Batchstep structure
It is always composed of 2 attributes:
- method="io.github.mzmine.modules.somethingElse"
- parameter_version="1" or "2"

I don't currently know what 1 and 2 means

I should scout in the modules directory in mzmine to understand how to retrieve the modules and/or exploit some structures.
-> https://github.com/mzmine/mzmine.github.io

First batchstep should always be file input? (idk but feels right), then it's up to the user.

        
## Todo tomorrow, one day ... or never

Look if it makes sense to parse a complete XML file and then modify by removing what's not selected by the user.
- Not all parameters need to be set, some are there because the method allows it.

I believe it should be good to create a general batchstep object which gets filled with the needed information (look into Luca's rust presetantion for not using a constructor (I forgot the right term but it's when you use points under the ::default() definition)).
- something like this has already been created in main.rs in sandbox git but it should be expanded to fit all parameters.

###
###
###


## Today I learned that

-