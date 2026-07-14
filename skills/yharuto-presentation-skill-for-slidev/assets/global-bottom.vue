<!--
  Page counter, equivalent to Marp's `paginate: true`.
  Slidev has no built-in page-number toggle, so this uses the global-layers
  mechanism (a file named global-bottom.vue at the project root renders
  below every slide). Hidden on the cover slide, matching how Marp decks
  conventionally omit the page number from the title slide.
-->
<template>
  <div
    v-if="$nav.currentLayout !== 'cover'"
    class="absolute bottom-2 right-4 text-sm"
    style="color: #999999; font-family: var(--font-default, 'Noto Sans JP', sans-serif);"
  >
    {{ $nav.currentPage }} / {{ $nav.total }}
  </div>
</template>
