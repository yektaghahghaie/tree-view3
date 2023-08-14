<script>
export default {
  name: "TreeView",
  props: {
    node: {
      type: Object,
      required: true,
    },
  },
  methods: {
    addFolder() {
      const label = prompt("Enter folder name");
      if (!label) return;
      const newFolder = { id: Date.now(), label, children: [] };
      this.node.children.push(newFolder);
    },
    addFile() {
      const label = prompt("Enter file name");
      if (!label) return;
      const newFile = { id: Date.now(), label };
      this.node.children.push(newFile);
    },
    deleteNode() {
      this.$emit("deleteNode", this.node);
    },
  },
};
</script>

<template>
  <div class="node">
    <div class="node__details">
      <div class="node__label">
        {{ node.label }}
        <small v-if="!node.children">(file)</small>
      </div>
      <div class="node__actions">
        <button v-if="node.children" @click="addFolder">Add Folder</button>
        <button v-if="node.children" @click="addFile">Add File</button>
        <button v-if="node.id !== 1" @click="deleteNode">Delete</button>
      </div>
    </div>
    <div v-if="node.children" class="node__children">
      <TreeView
        v-for="child in node.children"
        :key="child.id"
        :node="child"
        @addFolder="$emit('addFolder', $event)"
        @addFile="$emit('addFile', $event)"
        @deleteNode="$emit('deleteNode', $event)"
      />
    </div>
  </div>
</template>

<style lang="scss">
.node {
  padding: 8px 0 8px 8px;
  border-left: 2px solid #ddd;

  &:hover {
    border-color: #0ea4ea;
  }

  &__details {
    display: flex;
    align-items: center;
    margin-bottom: 10px;

    &:hover {
      background-color: #eee;
    }
  }

  &__label {
    flex: 1;
  }

  &__actions {
    button {
      cursor: pointer;
      margin-left: 8px;
    }
  }

  &__children {
    margin-left: 16px;
  }
}
</style>
