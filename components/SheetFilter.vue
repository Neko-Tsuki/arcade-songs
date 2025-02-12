<script setup lang="ts">
import { inject, Ref } from '@nuxtjs/composition-api';
import type { Filters, FilterOptions } from '~/types';

const filters: Ref<Filters> = inject('filters')!;
const filterOptions: Ref<FilterOptions> = inject('filterOptions')!;
</script>

<template>
  <div>
    <v-row>
      <v-col
        v-if="filterOptions.categories != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-select
          v-model="filters.categories"
          :items="filterOptions.categories"
          prepend-icon="mdi-folder-multiple-outline"
          :label="$t('term.category')"
          :placeholder="$t('ui.all')"
          persistent-placeholder
          multiple
          chips
          deletable-chips
          clearable
        />
      </v-col>
      <v-col
        v-if="filterOptions.titles != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-combobox
          v-model="filters.title"
          :items="filterOptions.titles"
          prepend-icon="mdi-alphabetical-variant"
          :label="$t('term.title')"
          :placeholder="$t('ui.all')"
          persistent-placeholder
          clearable
        />
      </v-col>
      <v-col
        v-if="filterOptions.difficulties != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-select
          v-model="filters.difficulties"
          :items="filterOptions.difficulties"
          prepend-icon="mdi-skull-outline"
          :label="$t('term.difficulty')"
          :placeholder="$t('ui.all')"
          persistent-placeholder
          multiple
          chips
          deletable-chips
          clearable
        />
      </v-col>
      <v-col
        v-if="filterOptions.levels != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <div class="d-flex flex-grow-1 align-end">
          <v-select
            v-model="filters.minLevelValue"
            :items="(!filters.useInternalLevel ? filterOptions.levels : filterOptions.internalLevels)"
            prepend-icon="mdi-numeric-9-plus-box-multiple-outline"
            :label="(!filters.useInternalLevel ? $t('term.minLevel') : $t('term.minInternalLevel'))"
            :placeholder="(((!filters.useInternalLevel ? filterOptions.levels : filterOptions.internalLevels) || [])[0] || { text: '?' }).text"
            persistent-placeholder
            clearable
          />
        </div>
        <div class="d-flex flex-grow-1 align-end pl-6">
          <v-select
            v-model="filters.maxLevelValue"
            :items="(!filters.useInternalLevel ? filterOptions.levels : filterOptions.internalLevels)"
            :label="(!filters.useInternalLevel ? $t('term.maxLevel') : $t('term.maxInternalLevel'))"
            :placeholder="(((!filters.useInternalLevel ? filterOptions.levels : filterOptions.internalLevels) || []).slice(-1)[0] || { text: '?' }).text"
            persistent-placeholder
            clearable
          />
        </div>
        <div
          v-if="filterOptions.internalLevels != null && filterOptions.internalLevels.length !== 0"
          class="d-flex flex-grow-0 align-center pl-6 align-self-stretch"
        >
          <v-btn
            icon
            @click="filters.useInternalLevel = !filters.useInternalLevel || null;"
          >
            <v-icon size="2.4em">
              {{ !filters.useInternalLevel ? 'mdi-closed-caption-outline' : 'mdi-closed-caption' }}
            </v-icon>
          </v-btn>
        </div>
      </v-col>
      <v-col
        v-if="filterOptions.versions != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-select
          v-model="filters.versions"
          :items="filterOptions.versions"
          prepend-icon="mdi-star-outline"
          :label="$t('term.version')"
          :placeholder="$t('ui.all')"
          persistent-placeholder
          multiple
          chips
          deletable-chips
          clearable
        />
      </v-col>
      <v-col
        v-if="filterOptions.types != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-select
          v-model="filters.types"
          :items="filterOptions.types"
          prepend-icon="mdi-assistant"
          :label="$t('term.type')"
          :placeholder="$t('ui.all')"
          persistent-placeholder
          multiple
          chips
          deletable-chips
          clearable
        />
      </v-col>
      <v-col
        v-if="filterOptions.artists != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-combobox
          v-model="filters.artist"
          :items="filterOptions.artists"
          prepend-icon="mdi-account-music-outline"
          :label="$t('term.artist')"
          :placeholder="$t('ui.all')"
          persistent-placeholder
          clearable
        />
      </v-col>
      <v-col
        v-if="filterOptions.noteDesigners != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-select
          v-model="filters.noteDesigners"
          :items="filterOptions.noteDesigners"
          prepend-icon="mdi-account-edit-outline"
          :label="$t('term.noteDesigner')"
          :placeholder="$t('ui.all')"
          persistent-placeholder
          multiple
          chips
          deletable-chips
          clearable
        />
      </v-col>
      <v-col
        v-if="filterOptions.regions != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-select
          v-model="filters.region"
          :items="filterOptions.regions"
          prepend-icon="mdi-map-search"
          :label="$t('term.region')"
          :placeholder="$t('ui.all')"
          persistent-placeholder
          clearable
        />
      </v-col>
      <v-col
        v-if="filterOptions.bpms != null"
        cols="12"
        md="6"
        class="d-flex align-end"
      >
        <v-row no-gutters>
          <v-col
            cols="6"
            class="d-flex align-end"
          >
            <v-text-field
              v-model.number="filters.minBPM"
              type="number"
              :min="0"
              prepend-icon="mdi-metronome"
              :label="$t('term.minBPM')"
              :placeholder="String(filterOptions.bpms[0] || 0)"
              persistent-placeholder
              clearable
            />
          </v-col>
          <v-col cols="6" class="d-flex align-end pl-6">
            <v-text-field
              v-model.number="filters.maxBPM"
              type="number"
              :min="0"
              :label="$t('term.maxBPM')"
              :placeholder="String(filterOptions.bpms.slice(-1)[0] || 999)"
              persistent-placeholder
              clearable
            />
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </div>
</template>
