<template>
    <div class="content-wrapper" v-if="!loading">
        <app-page-top-section :title="$t('org_structure')" >
            <app-default-button
                v-if="$can('create_department')"
                :title="$addLabel('department')"
                @click="openDepartmentModal()"
            />
        </app-page-top-section>
        <div class="card card-with-shadow border border-0 custom-scrollbar">
            <app-organization-chart :chart-data="chartData" :druggable="true" @selected-item="selectedItem"></app-organization-chart>
        </div>
        <app-department-modal
            v-if="isAddEditModalActive"
            v-model="isAddEditModalActive"
            :selected-url="selectedUrl"
        />
    </div>
    <app-overlay-loader v-else />
</template>

<script>
import {axiosGet} from "../../../../../common/Helper/AxiosHelper";
import {organizeOrganizationStructure} from "./Helper";

export default {
    name: "OrganizationStructure",
    data() {
        return {
            chartData: {},
            loading: true,
            isAddEditModalActive: false,
            selectedUrl: ''
        }
    },
    methods: {
        getOrganizationStructure() {
            axiosGet(this.apiUrl.ORGANIZATION_STRUCTURE).then(response => {
                this.chartData = organizeOrganizationStructure(response.data);
            }).finally(() => {
                this.loading = false;
            });
        },
        selectedItem(current) {
            this.isAddEditModalActive = true;
            this.selectedUrl = `${this.apiUrl.DEPARTMENTS}/${current.id}`
        },
        openDepartmentModal() {
            this.selectedUrl = '';
            this.isAddEditModalActive = true;
        },
    },
    watch: {
        isAddEditModalActive: {
            handler: function (value) {
                if (!value) {
                    this.getOrganizationStructure();
                }
            },
            immediate: true
        }
    }

}
</script>

<style scoped>

</style>
