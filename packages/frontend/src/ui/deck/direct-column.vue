<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<XColumn :column="column" :isStacked="isStacked" :refresher="() => reloadTimeline()">
	<template #header><i class="ti ti-mail" style="margin-right: 8px;"></i>{{ column.name || i18n.ts._deck._columns.direct }}</template>

	<MkNotesTimeline :paginator="paginator"/>
</XColumn>
</template>

<script lang="ts" setup>
import { markRaw, ref } from 'vue';
import XColumn from './column.vue';
import type { Column } from '@/deck.js';
import MkNotesTimeline from '@/components/MkNotesTimeline.vue';
import { i18n } from '@/i18n.js';
import { Paginator } from '@/utility/paginator.js';

defineProps<{
	column: Column;
	isStacked: boolean;
}>();

const paginator = markRaw(new Paginator('notes/mentions', {
	limit: 10,
	params: {
		visibility: 'specified',
	},
}));

function reloadTimeline() {
	return new Promise<void>((res) => {
		paginator.reload().then(() => {
			res();
		});
	});
}
</script>
