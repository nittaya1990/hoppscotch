<template>
  <AppSection id="script" :label="$t('preRequest.script')">
    <div
      class="
        bg-primary
        border-b border-dividerLight
        flex flex-1
        top-upperSecondaryStickyFold
        pl-4
        z-10
        sticky
        items-center
        justify-between
      "
    >
      <label class="font-semibold text-secondaryLight">
        {{ $t("preRequest.javascript_code") }}
      </label>
      <div class="flex">
        <ButtonSecondary
          v-tippy="{ theme: 'tooltip' }"
          to="https://docs.hoppscotch.io/features/pre-request-script"
          blank
          :title="$t('app.wiki')"
          svg="help-circle"
        />
        <ButtonSecondary
          v-tippy="{ theme: 'tooltip' }"
          :title="$t('action.clear')"
          svg="trash-2"
          @click.native="clearContent"
        />
      </div>
    </div>
    <div class="border-b border-dividerLight flex">
      <div class="border-r border-dividerLight w-2/3">
        <SmartJsEditor
          v-model="preRequestScript"
          :options="{
            maxLines: Infinity,
            minLines: 16,
            autoScrollEditorIntoView: true,
            showPrintMargin: false,
            useWorker: false,
          }"
          complete-mode="pre"
        />
      </div>
      <div
        class="
          bg-primary
          h-full
          top-upperTertiaryStickyFold
          min-w-46
          max-w-1/3
          p-4
          z-9
          sticky
          overflow-auto
        "
      >
        <div class="text-secondaryLight pb-2">
          {{ $t("helpers.pre_request_script") }}
        </div>
        <SmartAnchor
          :label="$t('preRequest.learn')"
          to="https://docs.hoppscotch.io/features/pre-request-script"
          blank
        />
        <h4 class="font-bold text-secondaryLight pt-6">
          {{ $t("preRequest.snippets") }}
        </h4>
        <div class="flex flex-col pt-4">
          <TabSecondary
            v-for="(snippet, index) in snippets"
            :key="`snippet-${index}`"
            :label="snippet.name"
            active
            @click.native="useSnippet(snippet.script)"
          />
        </div>
      </div>
    </div>
  </AppSection>
</template>

<script lang="ts">
import { defineComponent } from "@nuxtjs/composition-api"
import { usePreRequestScript } from "~/newstore/RESTSession"
import preRequestScriptSnippets from "~/helpers/preRequestScriptSnippets"

export default defineComponent({
  setup() {
    const preRequestScript = usePreRequestScript()

    const useSnippet = (script: string) => {
      preRequestScript.value += script
    }

    const clearContent = () => {
      preRequestScript.value = ""
    }

    return {
      preRequestScript,
      snippets: preRequestScriptSnippets,
      useSnippet,
      clearContent,
    }
  },
})
</script>
