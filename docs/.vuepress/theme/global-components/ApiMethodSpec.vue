<template>
  <div class="api-method">
    <div class="method">{{ method.toUpperCase() }}</div>
    <div class="path">
      {{ host }}<strong>{{ path }}</strong>
    </div>

    <div v-if="needShow('summary')" class="api-method-summary">
      <strong>
        <slot name="summary">
          <p>
            {{ summary }}
          </p>
        </slot>
      </strong>
    </div>

    <div v-if="needShow('description')" class="api-method-description">
      <slot name="description">
        <p>
          {{ description }}
        </p>
      </slot>
    </div>

    <form class="api-method-spec tabs">
      <input type="radio" name="tabs" :id="`tabone${reqInputId}`" checked />
      <label :for="`tabone${reqInputId}`">Request</label>
      <div class="api-method-request tab">
        <div v-if="needShow('headers')" class="api-method-headers">
          <div class="parameters-description">Headers</div>
          <table>
            <tbody>
              <slot name="headers" />
            </tbody>
          </table>
        </div>

        <div v-if="needShow('pathParams')" class="api-method-path-parameters">
          <div class="parameters-description">Path Paramter</div>
          <table>
            <tbody>
              <slot name="pathParams" />
            </tbody>
          </table>
        </div>

        <div v-if="needShow('bodyParams')" class="api-method-body-parameters">
          <div class="parameters-description">Body Paramter</div>
          <table>
            <tbody>
              <slot name="bodyParams" />
            </tbody>
          </table>
        </div>

        <div v-if="needShow('queryParams')" class="api-method-query-parameters">
          <div class="parameters-description">Query Parameters</div>
          <table>
            <tbody>
              <slot name="queryParams" />
            </tbody>
          </table>
        </div>

        <div
          v-if="needShow('formDataParams')"
          class="api-method-form-data-parameters"
        >
          <div class="parameters-description">Form Data Parameters</div>
          <table>
            <tbody>
              <slot name="formDataParams" />
            </tbody>
          </table>
        </div>
      </div>

      <template v-if="needShow('response')">
        <input type="radio" name="tabs" :id="`tabtwo${resInputId}`" />
        <label :for="`tabtwo${resInputId}`">Response</label>
        <div class="api-method-response tab">
          <slot name="response" />
        </div>
      </template>
    </form>
  </div>
</template>

<script>
/**
 * ???????????? ::::: api-method ??????
 * props:
 *  method??????????????????get/post/put/patch
 *  host???????????????
 *  path????????????????????? URL ?????? host ????????????
 *  summary???????????????
 *  description????????????????????????
 *
 * slot:
 *  summary??????????????????????????????????????? props
 *  description???????????????????????????????????????????????? props
 *  headers????????????
 *  pathParams???path ??????
 *  bodyParams???body ??????
 *  queryParams???query ??????
 *  formDataParams???formData ??????
 */

const getRandomStr = () => {
  return Math.random().toString()
}

export default {
  name: 'ApiMethodSpec',
  props: {
    method: {
      type: String,
    },
    host: {
      type: String,
    },
    path: {
      type: String,
    },
    summary: {
      type: String,
    },
    description: {
      type: String,
    },
  },
  data() {
    return {
      reqInputId: getRandomStr(),
      resInputId: getRandomStr(),
    }
  },
  methods: {
    needShow(key) {
      return this[key] || this.$slots[key] || this.$scopedSlots[key]
    },
  },
}
</script>

<style></style>
