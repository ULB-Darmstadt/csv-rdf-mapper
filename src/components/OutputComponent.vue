<template>
  <div>
    <h4 class="title is-4">3. Generate and Display RDF-Graph</h4>
    <p class="content">Once you are done with adjusting the mapping between your shapegraph and nodegraph, you can
      generate your RDF-Graph by clicking the button below.
    </p>

    <button class="button is-dark content" @click="generateRDF()">Create Datagraph</button>
    <div v-if="rdfGraphGenerated" class="content">
      <button class="copy-btn" @click="copyToClipboard()">{{ copyButtonText }}</button>
      <pre is-family-code>{{ rdfGraph }}</pre>
    </div>
  </div>
</template>

<script>
import { graph, namedNode, serialize, literal } from 'rdflib';

import { RDF_PREDICATE_TYPE } from "@/constants";
import { ApplicationProfile } from '../NodeShape'
import { Mapping } from '../mapping'
import { InputData } from '@/inputdata';

  export default {
    props: {
      mapping: Mapping,
      dataInput: InputData,
      applicationProfile: ApplicationProfile,
    },
    data() {
        return {
          rdfGraphGenerated: false,
          rdfGraph: "",
          tripleStore: null,
          PathColumnMapping: null,
          ShapeCsvMapping: null,
          copyButtonText: 'Copy graph',
        }
    },
    methods: {
      copyToClipboard() {
        this.copyButtonText = '✓ Copied!';
        console.log("copyButtonText:", this.copyButtonText)
        setTimeout(() => {
          this.copyButtonText = 'Copy graph';
        }, 1000);
        if (navigator.clipboard && window.isSecureContext) {
          return navigator.clipboard.writeText(this.rdfGraph);
        } else {
          let textArea = document.createElement("textarea");
          textArea.value = this.rdfGraph;
          textArea.style.position = "fixed";
          document.body.appendChild(textArea);
          textArea.focus();
          textArea.select();
          try {
            document.execCommand('copy');
            textArea.remove();
          } catch (err) {
            console.error('Fallback: Oops, unable to copy', err);
          }
        }        
      },

      generateRDF() {
        const newTripleStore = graph();
        this.applicationProfile.getNodeShapes().forEach(shape => {
          const props = shape.getProperties()
          props.forEach(prop => {
            const match = this.mapping.getMapping(shape.nodeId.value, prop.path.value)
            const data = this.dataInput.data[match.dataFile]
            data.headers.forEach((header, header_index) => {
              if (header == match.dataColumn) {
                data.content.forEach((row, row_index) =>{
                  let subject = null
                  if(data.recordIds) {
                    subject = namedNode(shape.nodeId.value + data.recordIds[row_index])
                  }
                  else {
                    subject = namedNode(shape.nodeId.value + row[0])
                  }
                  newTripleStore.add(subject, RDF_PREDICATE_TYPE, shape.nodeId)
                  if(prop.node) {
                    const targetNodesRaw = row[header_index]
                    // Try splitting the string, if splitting fails, put the original string into a list
                    let targetNodesList = [];

                    try {
                      targetNodesList = targetNodesRaw.split(",");
                    } catch (err) {
                      targetNodesList = [targetNodesRaw];
                    }
                    targetNodesList.forEach(targetNode => {
                      newTripleStore.add(subject, prop.path, namedNode(prop.node.value + targetNode))
                    })
                  }
                  else {
                    newTripleStore.add(subject, prop.path, literal(row[header_index], prop.datatype))
                  }
                })
              }
            })
          })
        })
        serialize(undefined, newTripleStore, undefined, 'text/turtle', (err, str) => {
          if (err) {
            console.log('Serialization error:', err);
          } else {
            this.rdfGraph = str;
          }
        });
        this.rdfGraphGenerated = true;
      },

      validateGraph() {
        // To be added
      },
  }
}
</script>

<style scoped>
.content {
    position: relative;
}

.copy-btn {
    display: flex;
    flex-direction: column;
    white-space: nowrap;
    background-color: #e0e0e0;
    gap: 10px;
    padding: 12px;
    text-decoration: none;
    color: #686868;
    border-radius: 6px;
    border: 0;
    outline: none;
    font-size: 1rem;
    transition: all 0.2s ease-out;

    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
}

.copy-btn:hover {
    cursor: pointer;
    background-color: #d1d1d1;
    color: #686868;
}

</style>
