<template>
    <div>
        Here goes the commit graphs for {{ this.projects.map(function(project) { return project.name }) }}
    </div>
</template>

<script>
export default {
    props: {
        projects: {
            type: Array,
            default: []
        }
    },


}
</script>