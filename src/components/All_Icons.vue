<template>
    <div class="input_big">
        <input @keypress.enter="searchInput"
               id="myInput"
               class="gig-input"
               type="search"
               placeholder="search"
               v-model="search">
    </div>

    <div class="icon_box">
        <div class="icons_main" v-for="icon in filteredIcons()" :key="icon.id">
            <img :src="icon.url" style="width: 45px; margin: 20px; " alt="">
        </div>
    </div>
    <Pagination />








</template>

<script>

import axios from "axios";
import Pagination from "@/components/Pagination.vue";


export default {
    name: "All_Icons",
    components: {Pagination},

    data() {
        return {
            icons: [],
            search: '',
            currentPage: 1, // 3616 страниц
            pageSize: 60, // Размер страницы
            totalIcons: 0
        }

    },
    methods: {
        async getIcons() {
            const {data} = await axios.get("https://svg.q19.kz/api/v1/icons/", {
                params: {
                    limit: this.pageSize,
                    keyword: this.search.toLowerCase(),
                    page: this.currentPage
                }
            })
            console.log("res", data.data)
            this.icons = data.data
            this.totalIcons = data.total;
        },

        async searchInput(event) {
            this.currentPage = 1
            await this.getIcons();
            this.filteredIcons();
        },
        filteredIcons() {
            // this.getIcons()
            return this.icons
        },
        async paginationFunc(index) {
            this.currentPage = index
            console.log("Success", this.currentPage);
            await this.getIcons()
            this.filteredIcons()
        },

        // async paginationIcons() {
        //     console.log("Success", index)
        //     const {data} = await axios.get("https://svg.q19.kz/api/v1/icons/", {
        //         params: {
        //             limit: this.pageSize,
        //             keyword: this.search,
        //             page: index
        //         }
        //     })
        //     this.icons = data.data
        //     this.filteredIcons()
        // }

    },




    async created() {
        await this.getIcons()
        this.filteredIcons()
    },


}
</script>

<style scoped>


.icon_box {
    width: 1200px;
    height: max-content;
    border: none;
    margin-top: 20px;
    border-radius: 12px;
    display: flex;
    flex-wrap: wrap;
    margin-left: 120px;

}

.icons_main {
    background-color: snow;
    border: 3px solid darkgray;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100px;
    height: 100px;
    margin: 10px;
    border-radius: 15px;

}


</style>