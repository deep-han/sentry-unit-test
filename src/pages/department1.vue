<template>
  <section>
    <el-button-group>
      <el-button
        type="primary"
        v-for="(item, index) in referenceErrors"
        :key="index"
        @click="handleError('referenceErrors', index)"
        >引用错误{{ index }}</el-button
      >
    </el-button-group>
    <el-button-group>
      <el-button
        type="primary"
        v-for="(item, index) in rangeErrors"
        :key="index"
        @click="handleError('rangeErrors', index)"
        >范围错误{{ index }}</el-button
      >
    </el-button-group>

    <section>
      <p v-for="item in logs" :key="item.timestamp">
        {{ item.timestamp | formatDate('') }}: {{ item.msg }}
      </p>
    </section>
  </section>
</template>

<script>
import {formatDate} from '@/const/filter.js'
export default {
  data() {
    return {
      logs: [],
      referenceErrors: [
        {
          msg: 'Uncaught ReferenceError: a is not defined',
          fn: () => a()
        },
        {
          msg: 'Uncaught ReferenceError: b is not defined',
          fn: () => console.log(b)
        }
      ],
      rangeErrors: [
        {
          msg: 'Uncaught RangeError: Invalid array length',
          fn: () => ([].length = -5)
        },
        {
          msg:
            'Uncaught RangeError: toFixed() digits argument must be between 0 and 20 at Number.toFixed',
          fn: () => console.log(num.toFixed(-1))
        }
      ]
    }
  },
  methods: {
    handleError(type, index) {
      const timestamp = +new Date()
      const {fn, msg} = this[type][index]
      this.logs.unshift({
        timestamp,
        msg
      })
      fn()
    }
  },
  filters: {
    formatDate: formatDate
  }
}
</script>

<style lang="less"></style>
