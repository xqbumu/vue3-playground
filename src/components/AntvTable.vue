<template>
  Input:
  <input v-model="text" />
  <br />
  Result: {{ text }}
  <br />
  state: {{ state }}
  <a-table
    :columns="columns"
    :data-source="data"
    :custom-row="customRow"
  >
    <!-- :row-selection="rowSelection" -->
    <template #name="{ text }">
      <a>{{ text }}</a>
    </template>
  </a-table>
</template>

<script lang="ts">
import { computed, customRef, defineComponent, reactive, h } from "vue";
import { Checkbox } from "ant-design-vue"

interface DataItem {
  key: number;
  name: string;
  age: number;
  address: string;
  children?: DataItem[];
}

const data: DataItem[] = [
  {
    key: 1,
    name: "John Brown sr.",
    age: 60,
    address: "New York No. 1 Lake Park",
    children: [
      {
        key: 11,
        name: "John Brown",
        age: 42,
        address: "New York No. 2 Lake Park",
      },
      {
        key: 12,
        name: "John Brown jr.",
        age: 30,
        address: "New York No. 3 Lake Park",
        children: [
          {
            key: 121,
            name: "Jimmy Brown",
            age: 16,
            address: "New York No. 3 Lake Park",
          },
        ],
      },
      {
        key: 13,
        name: "Jim Green sr.",
        age: 72,
        address: "London No. 1 Lake Park",
        children: [
          {
            key: 131,
            name: "Jim Green",
            age: 42,
            address: "London No. 2 Lake Park",
            children: [
              {
                key: 1311,
                name: "Jim Green jr.",
                age: 25,
                address: "London No. 3 Lake Park",
              },
              {
                key: 1312,
                name: "Jimmy Green sr.",
                age: 18,
                address: "London No. 4 Lake Park",
              },
            ],
          },
        ],
      },
    ],
  },
  {
    key: 2,
    name: "Joe Black",
    age: 32,
    address: "Sidney No. 1 Lake Park",
  },
  {
    key: 3,
    name: "Checked User",
    age: 32,
    address: "Sidney No. 1 Lake Park",
  },
  {
    key: 4,
    name: "Disabled User",
    age: 99,
    address: "Sidney No. 1 Lake Park",
  },
  {
    key: 5,
    name: "Indeterminate User",
    age: 99,
    address: "Sidney No. 1 Lake Park",
  },
];

function useDebouncedRef(value, delay = 2000) {
  let timeout: number;
  return customRef((track, trigger) => {
    return {
      get() {
        track();
        return value;
      },
      set(newValue) {
        clearTimeout(timeout);
        timeout = setTimeout(() => {
          value = newValue;
          trigger();
        }, delay);
      },
    };
  });
}

export default defineComponent({
  setup() {
    const state = reactive({
      selectedRowKeys: [],
    });

    const columns = [
      {
        title: () => h(Checkbox),
        dataIndex: "name",
        key: "selection-column",
        className: ["ant-table-selection-column"],
        customRender: () => h(Checkbox),
      },
      {
        title: "Name",
        dataIndex: "name",
        key: "name",
        slots: { customRender: "name" },
      },
      {
        title: "Age",
        dataIndex: "age",
        key: "age",
        width: "12%",
      },
      {
        title: "Address",
        dataIndex: "address",
        width: "30%",
        key: "address",
      },
    ];

    const selectRow = (record) => {
      const selectedRowKeys = [...state.selectedRowKeys];
      if (selectedRowKeys.indexOf(record.key) >= 0) {
        selectedRowKeys.splice(selectedRowKeys.indexOf(record.key), 1);
      } else {
        selectedRowKeys.push(record.key);
      }
      state.selectedRowKeys = selectedRowKeys;
    };

    const rowSelection = computed(() => {
      return {
        getCheckboxProps: (record: DataItem) => {
          let res = {
            name: record.name,
          };
          if (record.name === "Disabled User") {
            res["disabled"] = true;
          } else if (record.name === "Indeterminate User") {
            res["indeterminate"] = true;
          } else if (record.name === "Checked User") {
            res["checked"] = true;
          }
          return res;
        },
        selectedRowKeys: state.selectedRowKeys,
        onChange: (
          selectedRowKeys: (string | number)[],
          selectedRows: DataItem[]
        ) => {
          state.selectedRowKeys = selectedRowKeys;
          console.log({ selectedRowKeys, selectedRows });
        },
        onSelect: (
          record: DataItem,
          selected: boolean,
          selectedRows: DataItem[]
        ) => {
          console.log({ record, selected, selectedRows });
        },
        onSelectAll: (
          selected: boolean,
          selectedRows: DataItem[],
          changeRows: DataItem[]
        ) => {
          console.log(selected, selectedRows, changeRows);
        },
      };
    });

    const customRow = (record) => {
      return {
        onClick: () => {
          selectRow(record);
        },
      };
    };
    return {
      data,
      columns,
      state,
      customRow,
      rowSelection,
      text: useDebouncedRef("hello"),
    };
  },
});
</script>