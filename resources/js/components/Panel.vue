<template>
    <div>
        <slot>
            <heading :level="1" class="mb-3">{{ panel.name }}</heading>
        </slot>

        <card class="mb-6 py-3 px-6 nova-grid-card-styles" :class="{
            'flex' : fields.some((field) => field.gridder),
            'flex-wrap' : fields.some((field) => field.gridder)
        }">
            <component
                :key="index"
                v-for="(field, index) in fields"
                :is="resolveComponentName(field)"
                :resource-name="resourceName"
                :resource-id="resourceId"
                :resource="resource"
                :field="field"
                :class="fieldClass(field)"
                @actionExecuted="actionExecuted"
            />

            <div
                v-if="shouldShowShowAllFieldsButton"
                class="bg-20 -mt-px -mx-6 -mb-6 border-t border-40 p-3 text-center rounded-b text-center"
            >
                <button
                    class="block w-full dim text-sm text-80 font-bold"
                    @click="showAllFields"
                >
                    {{ __('Show All Fields') }}
                </button>
            </div>
        </card>
    </div>
</template>

<script>
    import { BehavesAsPanel } from 'laravel-nova'

    export default {
        mixins: [BehavesAsPanel],

        methods: {
            fieldClass(field) {
                return field.gridder ? field.gridder.panelSize : 'w-full'
            },
            /**
             * Resolve the component name.
             */
            resolveComponentName(field) {
                return field.prefixComponent
                    ? 'detail-' + field.component
                    : field.component
            },

            /**
             * Show all of the Panel's fields.
             */
            showAllFields() {
                return (this.panel.limit = 0)
            },
        },

        computed: {
            /**
             * Limits the visible fields.
             */
            fields() {
                if (this.panel.limit > 0) {
                    return this.panel.fields.slice(0, this.panel.limit)
                }

                return this.panel.fields
            },

            /**
             * Determines if should display the 'Show all fields' button.
             */
            shouldShowShowAllFieldsButton() {
                return this.panel.limit > 0
            },
        },
    }
</script>

<style lang="scss">

.nova-grid-card-styles {
  display: flex; //ADDED
  flex-wrap: wrap; //ADDED
  width: 270%; //ADDED
}


</style>