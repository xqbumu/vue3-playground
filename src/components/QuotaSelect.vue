<template>
  <a-popover v-model:visible="popoverShow" trigger="click" placement="bottomLeft">
    <template #title>
      <a-input-search
        v-model:value="search"
        placeholder="input search text"
        style="width: 400px"
        @search="onSearch"
      />
    </template>
    <template #content>
      <a-radio-group v-model:value="selected" size="default" style="width: 100%">
        <a-row v-for="group in quotaGroup">
          <a-col :span="24">{{ group.name }}</a-col>
          <a-col v-for="quota in group.quotas" :span="12">
            <div :class="[$style.quota_item, $style.active]">{{ quota.name }}</div>
          </a-col>
          <a-divider />
        </a-row>
      </a-radio-group>
    </template>
    <a-button>
      <template #icon>
        <FundOutlined />
      </template>
      {{ selected }}
    </a-button>
  </a-popover>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { FundOutlined } from '@ant-design/icons-vue';

interface Quota {
  key: string;
  name: string;
}

interface QuotaGroup {
  name: string
  quotas: (Quota)[];
}

const quotaGroup: QuotaGroup[] = [
  {
    name: '浙江', quotas: [
      { key: 'hangzhou', name: '杭州' },
      { key: 'ningbo', name: '宁波' },
      { key: 'shaoxing', name: '绍兴' },
    ],
  },
  {
    name: '四川', quotas: [
      { key: 'chengdu', name: '成都' },
      { key: 'nanchong', name: '南充' },
    ],
  },
];

export default defineComponent({
  components: {
    FundOutlined
  },
  setup() {
    const popoverShow = ref<boolean>(true);
    const selected = ref<string>('hangzhou');
    const search = ref<string>('');

    const onSearch = (searchValue: string) => {
      console.log('use value', searchValue);
      console.log('or use this.value', search.value);
    };

    return {
      popoverShow,
      quotaGroup,
      selected,
      search,
      onSearch
    };
  },
});
</script>

<style module>
.quota_item {
  padding: "0px 8px 0 8px";
}

.active {
  background: "rgb(190, 200, 200)";
}
</style>
  