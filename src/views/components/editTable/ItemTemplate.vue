<template>
  <div>
    <b>#{{item.code}}</b>
    <span>{{ item.name }}</span>
    <abbr>{{item.name}}</abbr>
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
