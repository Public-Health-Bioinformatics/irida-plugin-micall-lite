# IRIDA MiCall-Lite Pipeline Plugin

![galaxy-workflow-diagram][]

# Table of Contents

   * [IRIDA MiCall-Lite Pipeline Plugin](#irida-micall-lite-pipeline-plugin)
   * [Installation](#installation)
      * [Installing to IRIDA](#installing-to-irida)
      * [Installing Galaxy Dependencies](#installing-galaxy-dependencies)
   * [Usage](#usage)
      * [Monitoring Pipeline Status](#monitoring-pipeline-status)
      * [Analysis Results](#analysis-results)
      * [Metadata Table](#metadata-table)
   * [Building/Packaging](#buildingpackaging)
      * [Installing IRIDA to local Maven repository](#installing-irida-to-local-maven-repository)
      * [Building the plugin](#building-the-plugin)
   * [Dependencies](#dependencies)

# Installation

## Installing to IRIDA

Please download the provided `irida-plugin-micall-lite-[version].jar` from the [releases][] page and copy to your `/etc/irida/plugins` directory.  Now you may start IRIDA and you should see the pipeline appear in your list of pipelines.

*Note:* This plugin requires you to be running IRIDA version >= `19.01`. Please see the [IRIDA][] documentation for more details.

## Installing Galaxy Dependencies

In order to use this pipeline, you will also have to install several Galaxy tools within your Galaxy instance. These can be found at:

| Name                 | Version           | Owner                          | Metadata Revision | Galaxy Toolshed Link                                                                                                    |
|----------------------|-------------------|--------------------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------|
| micall_lite          |  `0.1rc5+galaxy0` | `public-health-bioinformatics` | 4 (2020-02-28)    | [micall_lite-4:27d61a7f82f1](https://toolshed.g2.bx.psu.edu/view/public-health-bioinformatics/micall_lite/27d61a7f82f1) |
| fastp                |  `0.19.5+galaxy1` |                          `iuc` | 8 (2019-05-28)    | [fastp-8:1d8fe9bc4cb0](https://toolshed.g2.bx.psu.edu/view/iuc/fastp/1d8fe9bc4cb0)                                      |


## Setting up your MiCall-Lite `projects.json` File

MiCall-Lite requires a `projects.json` file that defines specific genomic regions and sequences to align against.

An example `projects.json` file is available on the [MiCall-Lite GitHub Repo](https://github.com/PoonLab/MiCall-Lite/blob/master/micall/projects.json)

The file is structured as follows:
```
{
  "projects": {
    "ERCC": {
      "max_variants": 0,
      "description": "External RNA Controls Consortium reference; https://doi.org/10.1186/1471-2164-6-150",
      "regions": [
        {
          "coordinate_region": "ERCC-00002",
          "seed_region_names": [
            "ERCC-00002-seed"
          ]
        },
        ...
        "HCV": {
              "max_variants": 0,
              "description": "Hepatitis C virus, coding regions",
              "regions": [
                {
                  "coordinate_region": "HCV1A-H77-Core",
                  "seed_region_names": [
                    "HCV-1a",
                    "HCV-1c",
                    "HCV-1e",
                    "HCV-1g"
                  ]
                },
        ...
        "HIV": {
              "max_variants": 0,
              "description": "Human immunodeficiency virus type 1, coding regions",
              "regions": [
                {
                  "coordinate_region": "GP41",
                  "seed_region_names": [
                    "HIV1B-env-seed"
                  ]
                },
        ...
        },
    "regions": {
        "ERCC-00002": {
          "is_nucleotide": false,
          "reference": [
            "PDYFHFRPSCSQYTGVGIQTVG*SWFY*ARLAYEHYGQ*FLEE*VPRKKRTFGFQSCTVALNSDR",
            "SQKNEI*AYGRSEWHKACSVSWHKIPCLDVIHVSGNCLVMRLFPGVRAAGICCKEGR*VRPTSLP",
            "PFSLLGPVSQFSEVPPYAEDHLKRASSLFVVRRLLVWRRIARIINCAVRAASEEVCCGFALTAGR",
            "RHNDSDSVSGDLHMFAAYFRWALASFRSQNRAIIPVLIYWTRNVGPSVVRIPRRLRAVYTLLSND",
            "CTTCDHLIQNYQSSSPRSGLVRTAAFARVCGL*LFSLDGLAHIWLTRRIVAIHRFARQSVLVGVR",
            "PRDSWLNGRTTRQPVLAFYP*KKKKKKK"
          ],
          "seed_group": null
        },
        ...
    }
}
```

## Preparing the 'micall_lite_projects_files' Tool Data Table in Galaxy

This workflow requires that the MiCall-Lite `projects.json` files described above are made available via a 
[Galaxy Tool Data Table](https://galaxyproject.org/admin/tools/data-tables/) called `micall_lite_projects_files`.
We recommend that the galaxy administrator use the [`data_manager_manual`](https://github.com/galaxyproject/tools-iuc/tree/master/data_managers/data_manager_manual) 
tool to manage that data table.

# Building/Packaging

Building and packaging this code is accomplished using [Apache Maven][maven]. However, you will first need to install [IRIDA][] to your local Maven repository. The version of IRIDA you install will have to correspond to the version found in the `irida.version.compiletime` property in the [pom.xml][] file of this project. Right now, this is IRIDA version `19.01.3`.

## Installing IRIDA to local Maven repository

To install IRIDA to your local Maven repository please do the following:

1. Clone the IRIDA project

```bash
git clone https://github.com/phac-nml/irida.git
cd irida
```

2. Checkout appropriate version of IRIDA

```bash
git checkout 19.01.3
```

3. Install IRIDA to local repository

```bash
mvn clean install -DskipTests
```

## Building the plugin

Once you've installed IRIDA as a dependency, you can proceed to building this plugin. Please run the following commands:

```bash
cd irida-plugin-example

mvn clean package
```

Once complete, you should end up with a file `target/irida-plugin-micall-lite-<version>.jar` which can be installed as a plugin to IRIDA.

If you have previously [setup IRIDA][irida-setup] before you may copy this JAR file to `/etc/irida/plugins` and restart IRIDA.  The plugin should now show up in the **Analyses > Pipelines** section of IRIDA.

![plugin-pipeline.png][]  

You should be able to run a pipeline with this plugin and get analysis results.

![plugin-results.png][]

# Dependencies

The following dependencies are required in order to make use of this plugin.

* [IRIDA][] >= 0.23.0
* [Java][] >= 1.8 and [Maven][maven] (for building)

## 6. Distribute

Once you've successfully built your plugin, you can distribute the JAR file to other IRIDA users to install in their instances.

[maven]: https://maven.apache.org/
[IRIDA]: http://irida.ca/
[Galaxy]: https://galaxyproject.org/
[Java]: https://www.java.com/
[irida-pipeline]: https://irida.corefacility.ca/documentation/developer/tools/pipelines/
[irida-pipeline-galaxy]: https://irida.corefacility.ca/documentation/developer/tools/pipelines/#galaxy-workflow-development
[irida-wf-ga2xml]: https://github.com/phac-nml/irida-wf-ga2xml
[pom.xml]: pom.xml
[workflows-dir]: src/main/resources/workflows
[workflow-structure]: src/main/resources/workflows/0.1.0/irida_workflow_structure.ga
[micalllite-plugin-java]: src/main/java/ca/corefacility/bioinformatics/irida/plugins/MicallLitePlugin.java
[irida-plugin-java]: https://github.com/phac-nml/irida/tree/development/src/main/java/ca/corefacility/bioinformatics/irida/plugins/IridaPlugin.java
[irida-updater]: src/main/java/ca/corefacility/bioinformatics/irida/plugins/ExamplePluginUpdater.java
[irida-setup]: https://irida.corefacility.ca/documentation/administrator/index.html
[properties]: https://en.wikipedia.org/wiki/.properties
[messages]: src/main/resources/workflows/0.1.0/messages_en.properties
[maven-min-pom]: https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#Minimal_POM
[pf4j-start]: https://pf4j.org/doc/getting-started.html
[plugin-results.png]: doc/images/plugin-results.png
[plugin-pipeline.png]: doc/images/plugin-pipeline.png
[plugin-metadata.png]: doc/images/example-plugin-metadata.png
[pipeline-parameters.png]: doc/images/pipeline-parameters.png
[plugin-save-results.png]: doc/images/example-plugin-save-results.png
[galaxy-workflow-diagram]: doc/images/galaxy-workflow-diagram.png
