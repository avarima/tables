<template>
	<div :class="{ empty: localValue === '' }">
		<NcTextField
			:value.sync="localValue"
			:label="t('tables', 'Search')"
			trailing-button-icon="close"
			:show-trailing-button="localValue !== ''"
			@trailing-button-click="clearValue"
			@update:value="debounceSubmit">
			<Magnify :size="16" />
		</NcTextField>
	</div>
</template>

<script>
import { NcTextField } from '@nextcloud/vue'
import Magnify from 'vue-material-design-icons/Magnify.vue'
import debounce from 'debounce'

export default {

	components: {
		Magnify,
		NcTextField,
	},

	props: {
		searchString: {
		      type: String,
		      default: null,
		    },
		columns: {
		      type: Array,
		      default: null,
		    },
	},

	data() {
		return {
			localValue: '',
		}
	},

	methods: {
		debounceSubmit: debounce(function() {
			this.submit()
		}, 500),

		clearValue() {
			this.localValue = ''
			this.submit()
		},
		submit() {
			this.$emit('set-search-string', this.localValue)
		},
	},

}
</script>
<style scoped>

:deep(.input-field input) {
	border-color: var(--color-primary-element);
}

.empty :deep(.input-field input) {
	border-color: transparent;
}

</style>
