<template>
  <span>
    <button
      class="release_button"
      :disabled="!getWorkload(workload).selected_tag.tag || getWorkload(workload).status == statuses.releasing"
      @click="release(workload)"
    >{{getWorkload(workload).status == statuses.releasing ? 'Relasing' : 'Release'}}</button>
  </span>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";
import { StoreNamespaces } from "../../store/types/StoreNamespaces";
import { Getter, Action } from "vuex-class";
import { WorkloadStatuses } from "../../store/types/Workloads/WorkloadStatuses";

@Component({})
export default class WorkloadRelease extends Vue {
  @Prop() protected workload: any;

  protected statuses = WorkloadStatuses;

  @Action("releaseVersion", { namespace: StoreNamespaces.workloads })
  public releaseVersion: any;

  @Getter("getWorkload", { namespace: StoreNamespaces.workloads })
  public getWorkload: any;

  public release() {
    const workload = this.getWorkload(this.workload);
    const releaseData: any = {
      Workload: workload.id,
      Container: workload.container,
      Current: workload.image + ":" + workload.current_tag.tag,
      Target: workload.image + ":" + workload.selected_tag.tag
    };

    this.releaseVersion({ workload: this.workload, releaseData });
  }
}
</script>

<style lang="scss">
@import "../../assets/scss/include";
.release_button {
  background: #343d5d;
  padding: 8px;
  color: #fff;
  border-radius: 5px;
  font-size: 11px;
  &:hover {
    background: #5c6484;
    cursor: pointer;
  }
  &:focus {
    outline: 0;
  }
  &:disabled {
    cursor: default;
    background: #f0f0fa;
    color: #948f8f;
  }
}
</style>