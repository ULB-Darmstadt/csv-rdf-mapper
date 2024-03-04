# CSV-RDF-Mapper

Converts tabular data (`csv`) to rdf (`turtle / text`) utilizing [SHACL](https://www.w3.org/TR/shacl/). The CSV-RDF-Mapper is an MVP to showcase a workflow to populate tabular data into a data graph. Current functionalities are mainly for showcase purposes and are not complete. A minimal shape graph and tabular data is included in the examples and embedded into the [Demo](https://ulb-darmstadt.github.io/csv-rdf-mapper/).

## Demo

[Live Demo](https://ulb-darmstadt.github.io/csv-rdf-mapper/)

## Project information

The tool was developed during the NFDI4Ing Seed Fund: [RDM-Workflows for construction engineering and architecture](https://doi.org/10.5281/zenodo.7802981).

## Features

- Conversion of `csv` data to `turtle / text` format using `shacl` for structuring.
- Example datasets and shape graph provided to demonstrate functionality in the [Demo](https://winroger.github.io/csv-rdf-mapper/).

## Planned Features

- Implement [rdf-validate-shacl](https://github.com/zazuko/rdf-validate-shacl) for validation
- Fix how nodeshapes are represented in the datagraph (see f.e. `n0:`)
- Extend functionalities how Shapegraph are populated, currently only supporting `nodeshapes` and `nodeShapeProperties` for demonstration.

## Technologies Used

- Vue.js (3.3.4)
- TypeScript (4.5.5)
- [rdflib.js](https://www.npmjs.com/package/rdflib)
- [papaparse](https://www.npmjs.com/package/papaparse)

## Installation

To install CSV-RDF-Mapper, follow these steps:
```console
git clone https://github.com/winroger/csv-rdf-mapper.git
cd csv-rdf-mapper
npm install
```

## Running the Application

To run the application locally, execute:
```console
npm run serve
```

## Building for Production

To build the project for production, use:
```console
npm run build
```

## TypeScript Classes

| Class             | Description |
|-------------------|-------------|
| NodeShape         | TypeScript class to populate the NodeShape class from a shape graph (turtle). |
| NodeShapeProperty | TypeScript class to automatically populate NodeShape Properties from a shape graph (turtle). |
| inputdata         | TypeScript class to populate a dataInput from a table (CSV). |
| mapping           | TypeScript class to create a mapping between CSV and NodeShapes, Column-headers, and Node Shape Properties. |
| constants         | TypeScript class that holds the needed prefixes |



## License

This project is distributed under the MIT License. See `LICENSE` for more information.

## Contact

- Winkler, Roger - [roger.winkler@tu-darmstadt.de](mailto:roger.winkler@tu-darmstadt.de) - [University and State Library](https://www.ulb.tu-darmstadt.de/) at the Technical University of Darmstadt, [Department Information Technology, Research and Development (III)](https://www.ulb.tu-darmstadt.de/die_bibliothek/ueberuns/organisation/abteilung_iii/index.en.jsp)
- Svatos-Raznjevic, Hana - [hana.svatos-raznjevic@icd.uni-stuttgart.de](mailto:hana.svatos-raznjevic@icd.uni-stuttgart.de) - [University of Stuttgart](https://www.uni-stuttgart.de/en/), [Institute for Computational Design and Construction](https://www.icd.uni-stuttgart.de)
