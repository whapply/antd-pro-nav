<!--
 * @Date: 2020-05-11 11:13:21
 * @Description: desc
 -->
<template>
  <div>
    <a-select default-value="lucy" style="width: 120px" @change="handleChange">
      <a-select-option value="jack">
        Jack1
      </a-select-option>
      <a-select-option value="lucy">
        Lucy2
      </a-select-option>
      <a-select-option value="disabled" disabled>
        Disabled3
      </a-select-option>
      <a-select-option value="Yiminghe">
        yiminghe3
      </a-select-option>
    </a-select>
    <a-select default-value="lucy" style="width: 120px" disabled>
      <a-select-option value="lucy">
        Lucy
      </a-select-option>
    </a-select>
    <a-select default-value="lucy" style="width: 120px" loading>
      <a-select-option value="lucy">
        Lucy
      </a-select-option>
    </a-select>
  </div>
</template>
<script>
export default {
  methods: {
    handleChange(value) {
      console.log(`selected ${value}`)
    }
  }
}
</script>
