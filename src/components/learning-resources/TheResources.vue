<template>
  <base-card>
    <!-- props and event listeners (e.g. @click) fall through to root html element -->
    <base-button
      @click="setSelectedTab('stored-resources')"
      :mode="storedResButtonMode"
    >Resources</base-button>
    <base-button
      @click="setSelectedTab('add-resource')"
      :mode="addResButtonMode"
    >Add Resource</base-button>
  </base-card>

  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import AddResource from './AddResource.vue'
import StoredResources from './StoredResources.vue'

export default {

components: {
    AddResource,
    StoredResources
  },

  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The Official Vue.js documentation',
          link: 'https://vuejs.org'
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn to google...',
          link: 'https://google.com'
        }
      ]
    }
  },
  provide () {
    return {
      resources: this.storedResources,
      addResource: this.addResource, // point to method
      deleteResource: this.removeResource // point to remove method
    }
  },
  computed: {
    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat'
    },
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat'
    }
  },
  methods: {
    addResource(title, description, url) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url
      }
      // put newest item at TOP of list with unshift (instead of push)
      this.storedResources.unshift(newResource)
    },
    removeResource(id) {
      const index = this.storedResources.findIndex(res => res.id === id)
      this.storedResources.splice(index, 1) // manipulate original array to make it reactive
    },
    setSelectedTab(tab) {
      this.selectedTab = tab
    }
  }
}
</script>