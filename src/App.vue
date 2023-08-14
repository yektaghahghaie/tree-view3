<script>
import TreeView from "./components/TreeView.vue";

export default {
  components: {
    TreeView,
  },
  data() {
    return {
      node: {
        id: 1,
        label: "root",
        children: [
          {
            id: 2,
            label: "child 1",
            children: [
              {
                id: 3,
                label: "child 1.1",
              },
              {
                id: 4,
                label: "child 1.2",
                children: [],
              },
            ],
          },
          {
            id: 5,
            label: "child 2",
            children: [],
          },
        ],
      },
    };
  },
  methods: {
    addFolder(node, label) {
      console.log("addFolder", node, label);
      // put your code here

      if (!node.children) {
        this.$set(node, "children", []);
      }
      node.children.push({ id: Date.now(), label, children: [] });
    },
    addFile(node, label) {
      console.log("addFile", node, label);
      // put your code here

      if (!node.children) {
        this.$set(node, "children", []);
      }
      node.children.push({ id: Date.now(), label, children: [] });
    },
    deleteNode(node) {
      console.log("deleteNode", node);
      // put your code here

      const parentNode = this.findParentNode(this.node, node);
      if (parentNode) {
        parentNode.children = parentNode.children.filter(
          (child) => child.id !== node.id
        );
      }
    },

    findParentNode(currentNode, targetNode) {
      if (currentNode.children && currentNode.children.length > 0) {
        for (const child of currentNode.children) {
          if (child === targetNode) {
            return currentNode;
          }
          const found = this.findParentNode(child, targetNode);
          if (found) {
            return found;
          }
        }
      }
      return null;
    },
  },
};
</script>

<template>
  <main>
    <h2>Tree View</h2>
    <TreeView
      :node="node"
      @addFolder="addFolder"
      @addFile="addFile"
      @deleteNode="deleteNode"
    />
  </main>
</template>

<style>
main {
  max-width: 600px;
  margin: 0 auto;
}
</style>










