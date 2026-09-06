# This is Michael's EMI open-notebook.

Today is 2024.07.05

## Todo today

### Have a look at the EMI discussion forum
    - https://github.com/orgs/earth-metabolome-initiative/discussions
###
###

## Doing
The idea is to create the parameters file using command line in the form of:
- cargo build argv1 argv2 
Where odds argvs are flagging which batchstep to include (inputted by user) and followed by it's parameter/s.

This are the modules:
- io.github.mzmine.modules.io.import_rawdata_all.AllSpectralDataImportModule
- io.github.mzmine.modules.dataprocessing.featdet_massdetection.MassDetectionModule
- io.github.mzmine.modules.dataprocessing.featdet_adapchromatogrambuilder.ModularADAPChromatogramBuilderModule
- io.github.mzmine.modules.dataprocessing.featdet_smoothing.SmoothingModule
- io.github.mzmine.modules.dataprocessing.featdet_chromatogramdeconvolution.minimumsearch.MinimumSearchFeatureResolverModule
- io.github.mzmine.modules.dataprocessing.filter_isotopegrouper.IsotopeGrouperModule
- io.github.mzmine.modules.dataprocessing.filter_rowsfilter.RowsFilterModule
- io.github.mzmine.modules.io.export_features_gnps.fbmn.GnpsFbmnExportAndSubmitModule
- io.github.mzmine.modules.io.export_features_sirius.SiriusExportModule

I believe the flags need to be the information after io.github.mzmine.modules. as it is constant between all modules.

