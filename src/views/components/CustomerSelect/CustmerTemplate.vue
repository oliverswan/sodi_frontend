<template>
  <div>
    <b>#{{item.contactName}}</b>
    <span>{{ item.contactName }}</span>
    <abbr>{{item.contactName}}</abbr>
  </div>
</template>

<script>
export default {
  props: {
    item: { required: true },
    searchText: { required: true }
  }
}
</script>
