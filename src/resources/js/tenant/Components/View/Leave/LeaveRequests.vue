<template>
    <div class="content-wrapper">
        <app-page-top-section :title="$t('leave_request')">
            <leave-top-buttons @open-model="openLeaveModal" :request-button="true"/>
        </app-page-top-section>

        <app-table
            :id="tableId"
            :options="options"
            @action="triggerActions"
        />

        <app-leave-create-edit-modal
            v-if="isLeaveModalActive"
            v-model="isLeaveModalActive"
            :tableId="tableId"
            :specificId="adminRequestId"
        />

        <app-leave-response-log-modal
            v-if="isResponseLogModalActive"
            v-model="isResponseLogModalActive"
            :url="logUrl"
            :table-id="tableId"
            :manager-dept="managerDept"
        />

        <app-confirmation-modal
            v-if="confirmationModalActive"
            :message="modalSubtitle"
            :modal-class="modalClass"
            :icon="modalIcon"
            modal-id="app-confirmation-modal"
            @confirmed="updateStatus"
            @cancelled="cancelled"
        />
    </div>
</template>

<script>
import LeaveRequestMixin from "../../Mixins/LeaveRequestMixin";
import LeaveRequestActionMixin from "../../Mixins/LeaveRequestActionMixin";
import LeaveTopButtons from "./Components/LeaveTopButtons";

export default {
    name: "LeaveRequest",
    mixins: [LeaveRequestMixin, LeaveRequestActionMixin],
    components: {LeaveTopButtons},
    props: {
        leaveId: {},
        managerDept: {}
    },
    created() {
        if (this.leaveId) {
            this.logUrl = `${this.apiUrl.LEAVES}/${this.leaveId}/log`
            this.isResponseLogModalActive = true;
        }
    }
}
</script>