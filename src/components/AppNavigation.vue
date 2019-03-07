<template>
    <span>
        <v-navigation-drawer app v-model="drawer" class="brown lighten-2" dark disable-resize-watcher>
            <v-list>
                <template v-for="(item, index) in items">
                    <v-list-tile :to="item.path">
                        <v-list-tile-content>
                            {{item.name}}
                        </v-list-tile-content>
                    </v-list-tile>
                    <v-divider :key="`divider-${index}`"></v-divider>
                </template>
            </v-list>
        </v-navigation-drawer>
        <v-toolbar app color="brown darken-4" dark>
            <v-toolbar-side-icon class="hidden-md-and-up" @click="drawer = !drawer"></v-toolbar-side-icon>
            <v-spacer class="hidden-md-and-up"></v-spacer>
            <v-toolbar-title>{{appTitle}}</v-toolbar-title>
            <v-spacer class="hidden-sm-and-down"></v-spacer>
	    <template v-for="(item, index) in items">
	        <v-btn color="brown lighten-3" class="hidden-sm-and-down" :to="item.path">{{item.name}}</v-btn>
	    </template>
        </v-toolbar>
    </span>
</template>

<script>
export default {
    name: 'AppNavigation',
    data() {
        return {
            appTitle: 'Douce Vue Demo',
            drawer: false,
            items: [
                { path: "/", name: "Home" },
                { path: "/about", name: "About" },
		{ path: '/contact', name: 'Contact' }
            ]
        };
    }
};
</script>

<style scoped>
</style>