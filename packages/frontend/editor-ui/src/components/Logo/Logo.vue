<script setup lang="ts">
import type { FrontendSettings } from '@n8n/api-types';
import { computed } from 'vue';

const props = defineProps<
	(
		| {
				location: 'authView';
		  }
		| {
				location: 'sidebar';
				collapsed: boolean;
		  }
	) & {
		releaseChannel: FrontendSettings['releaseChannel'];
	}
>();

const { location } = props;

const showLogoText = computed(() => {
	if (location === 'authView') return true;
	return !props.collapsed;
});

const $style = useCssModule();
const containerClasses = computed(() => {
	if (location === 'authView') {
		return [$style.logoContainer, $style.authView];
	}
	return [
		$style.logoContainer,
		$style.sidebar,
		props.collapsed ? $style.sidebarCollapsed : $style.sidebarExpanded,
	];
});
</script>

<template>
	<div :class="containerClasses" data-test-id="n8n-logo">
		<img src="/static/logo-futurenow.png" :class="$style.logo" />
		<slot />
	</div>
</template>

<style lang="scss" module>
.logoContainer {
	display: flex;
	justify-content: center;
	align-items: center;
}

.logo {
	width: 32px;
	height: 30px;
}

.logoText {
	margin-left: var(--spacing-5xs);
	path {
		fill: var(--color-text-dark);
	}
}

.authView {
	transform: scale(2);
	margin-bottom: var(--spacing-xl);

	.logo,
	.logoText {
		transform: scale(1.3) translateY(-2px);
	}

	.logoText {
		margin-left: var(--spacing-xs);
		margin-right: var(--spacing-3xs);
	}
}

.sidebarExpanded .logo {
	margin-left: var(--spacing-2xs);
}

.sidebarCollapsed .logo {
	width: 40px;
	height: 30px;
	padding: 0 var(--spacing-4xs);
}
</style>
