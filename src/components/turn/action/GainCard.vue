<template>
  <div class="actionBox col" data-bs-toggle="modal" data-bs-target="#modalGainCardHelp">
    <template v-if="isMostValueableShare">
      <span v-html="t('turnBot.action.gainCard.mostValuableShare')"></span>
    </template>
    <template v-else>
      <span v-html="t('turnBot.action.gainCard.matchSlot')"></span>
      <div class="selection">
        <AppIcon type="selection" name="card-selection" class="matrix"/>
        <table>
          <tr v-for="(row,indexRow) in action.cardSelectionMatrix" :key="indexRow">
            <td v-for="(col,indexCol) in row" :key="indexCol">
              <span v-if="col.includes(slot)">X</span>
            </td>
          </tr>
        </table>
      </div>
    </template>
  </div>

  <div class="modal" tabindex="-1" id="modalGainCardHelp">
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable modal-lg">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="staticBackdropLabel">{{t('turnBot.action.gainCard.help.title')}}</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" :aria-label="t('action.close')"></button>
        </div>
        <div class="modal-body">
          <p v-html="t('turnBot.action.gainCard.help.instruction')"></p>
          <ul>
            <li v-html="t('turnBot.action.gainCard.help.hint1')"></li>
            <li v-html="t('turnBot.action.gainCard.help.hint2')"></li>
            <li v-html="t('turnBot.action.gainCard.help.hint3')"></li>
            <li v-html="t('turnBot.action.gainCard.help.hint4')"></li>
          </ul>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">{{t('action.close')}}</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { CardAction } from '@/services/Card'
import CardSelection from '@/services/enum/CardSelection'
import Slot from '@/services/enum/Slot'
import { defineComponent, PropType } from 'vue'
import { useI18n } from 'vue-i18n'
import AppIcon from '../../structure/AppIcon.vue'

export default defineComponent({
  name: 'GainCard',
  setup() {
    const { t } = useI18n()
    return { t }
  },
  components: {
    AppIcon
  },
  props: {
    action: {
      type: Object as PropType<CardAction>,
      required: true
    },
    cardSlot: {
      type: String,
      required: true
    }
  },
  computed: {
    isMostValueableShare() : boolean {
      return this.action.cardSelection == CardSelection.MOST_VALUABLE_SHARE
    },
    slot() : Slot {
      return this.cardSlot as Slot
    }
  }
})
</script>

<style lang="scss" scoped>
.selection {
  display: inline-block;
  position: relative;
  margin-left: 1rem;
  width: 6rem;
  .matrix {
    width: 6rem;
  }
  table {
    position: absolute;
    left: 0.5rem;
    top: 0.25rem;
    z-index: 100;
    width: 5rem;
    height: 8.15rem;
    td {
      text-align: center;
      font-size: 1.1rem;
      font-weight: bold;
      width: calc(100%/3);
      height: calc(100%/4);
    } 
  }
}
</style>
