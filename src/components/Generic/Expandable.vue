<template>
    <div ref="self" class="hide-me" style="height: 0px">
        <slot></slot>
    </div>
</template>

<script>
	export default {
	name: "Expandable",
	props: {
		hidden: Boolean
	},
	watch: {
		hidden() {
			this.toggleExpanded();
		}
	},
	methods: {
		toggleExpanded: function() {
			var self = this.$refs.self;
			var cardHeight = self.scrollHeight;
			if (this.hidden) {
				var transition = self.style.transition;
				self.style.transition = '';
				requestAnimationFrame(function() {
					self.style.height = cardHeight + 'px';
					self.style.transition = transition;
					requestAnimationFrame(function() {
						self.style.height = 0 + 'px';
					});
				});
			}
			else {
				self.style.height = cardHeight + 'px';
				self.addEventListener('transitionend', this.handleTransitionEnd, )
			}
		},
		handleTransitionEnd: function () {
			var self = this.$refs.self;
			self.removeEventListener('transitionend', this.handleTransitionEnd);
			self.style.height = null;
		}
	}
}
</script>

<style lang="scss">
    .hide-me {
        overflow: hidden;
		transition: 0.2s ease-out;
		height: auto;
    }
</style>