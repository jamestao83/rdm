<script setup>
import { ref } from 'vue'
import { flatMap, reject } from 'lodash'
import Add from '../icons/Add.vue'
import Delete from '../icons/Delete.vue'
import IconButton from '../common/IconButton.vue'

const props = defineProps({
    value: Array,
})
const emit = defineEmits(['update:value'])

/**
 * @typedef Hash
 * @property {string} key
 * @property {string} [value]
 */
const kvList = ref([{ key: '', value: '' }])

/**
 *
 * @param {Hash[]} val
 */
const onUpdate = (val) => {
    val = reject(val, { key: '' })
    emit(
        'update:value',
        flatMap(val, (item) => [item.key, item.value])
    )
}
</script>

<template>
    <n-form-item :label="$t('element')" required>
        <n-dynamic-input
            v-model:value="kvList"
            :key-placeholder="$t('enter_field')"
            :value-placeholder="$t('enter_value')"
            preset="pair"
            @update:value="onUpdate"
        >
            <template #action="{ index, create, remove, move }">
                <icon-button v-if="kvList.length > 1" :icon="Delete" size="18" @click="() => remove(index)" />
                <icon-button :icon="Add" size="18" @click="() => create(index)" />
            </template>
        </n-dynamic-input>
    </n-form-item>
</template>

<style lang="scss" scoped></style>
