<template>
	<RowFormWrapper :title="column.title" :mandatory="column.mandatory" :description="column.description">
		<div class="align-center">
			<NcButton type="tertiary" :aria-label="t('tables', 'Reduce stars')" @click="less">
				<template #icon>
					<Minus :size="20" />
				</template>
			</NcButton>
			<div class="stars">
				{{ getStars }}
			</div>
			<NcButton type="tertiary" :aria-label="t('tables', 'Increase stars')" @click="more">
				<template #icon>
					<Plus :size="20" />
				</template>
			</NcButton>
		</div>
	</RowFormWrapper>
</template>

<script>
import { NcButton } from '@nextcloud/vue'
import RowFormWrapper from './RowFormWrapper.vue'
import Plus from 'vue-material-design-icons/Plus.vue'
import Minus from 'vue-material-design-icons/Minus.vue'

export default {
	components: {
		RowFormWrapper,
		NcButton,
		Plus,
		Minus,
	},
	props: {
		column: {
			type: Object,
			default: null,
		},
		value: {
			type: Number,
			default: null,
		},
	},
	data() {
		return {
		}
	},
	computed: {
		localValue: {
			get() {
				if (this.value !== null) {
					return this.value
				} else {
					if (this.column.numberDefault !== undefined) {
						this.$emit('update:value', this.column.numberDefault)
						return this.column.numberDefault
					} else {
						return null
					}
				}
			},
			set(v) { this.$emit('update:value', v) },
		},
		getStars() {
			return '★'.repeat(this.localValue) + '☆'.repeat(5 - this.localValue)
		},
	},
	methods: {
		more() {
			if (this.localValue < 5) {
				this.localValue++
			}
		},
		less() {
			if (this.localValue > 0) {
				this.localValue--
			}
		},
	},
}
</script>
<style lang="scss" scoped>

.align-center {
	align-items: center;
	display: inline-flex;
}

.stars {
	font-size: 1.4em;
	padding: 7px;
}

</style>
