<template>
  <div>
    <v-list nav class="navigation-menu">
      <v-list-item-group>
        <template v-for="(navItem, i) in navItems">
          <v-list-item
            v-if="navItem.type === 'item'"
            :key="i"
            :class="`navigation__${navItem.class}`"
            active-class="navigation-menu__item--active"
            :to="navItem.route"
          >
            <navigation-menu-item
              :show-tooltips="showTooltips"
              :text="navItem.text"
              :icon="navItem.icon"
            />
          </v-list-item>
          <v-list-group v-else-if="navItem.type === 'group'" :key="i">
            <template #activator>
              <navigation-menu-item
                :show-tooltips="showTooltips"
                :text="navItem.text"
                :icon="navItem.icon"
                :class="`navigation__${navItem.class}`"
              />
            </template>
            <v-list-item
              v-for="(subNavItem, si) in navItem.items"
              :key="si"
              :class="`navigation__${subNavItem.class}`"
              active-class="navigation-menu__item--active"
              :to="subNavItem.route"
            >
              <navigation-menu-item
                :show-tooltips="showTooltips"
                :text="subNavItem.text"
                :icon="subNavItem.icon"
              />
            </v-list-item>
          </v-list-group>
          <v-divider
            v-else-if="navItem.type === 'divider'"
            :key="i"
            class="mb-2"
          />
        </template>
      </v-list-item-group>
    </v-list>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import NavigationMenuItem from '@/components/NavigationMenuItem.vue';
import { Routes } from '@/router/routes';

type NavItemDetails = {
  readonly text: string;
  readonly route: string;
  readonly class?: string;
  readonly icon: string;
};

type NavItem = { readonly type: 'item' } & NavItemDetails;
type NavGroupItem = {
  readonly type: 'group';
} & NavItemDetails & { items: NavItem[] };
type DividerItem = { readonly type: 'divider' };
type MenuItem = NavItem | NavGroupItem | DividerItem;

@Component({
  components: { NavigationMenuItem }
})
export default class NavigationMenu extends Vue {
  @Prop({ required: false, default: false })
  showTooltips!: boolean;

  readonly navItems: MenuItem[] = [
    {
      type: 'item',
      text: this.$tc('navigation_menu.dashboard'),
      route: '/dashboard',
      class: 'dashboard',
      icon: 'mdi-monitor-dashboard'
    },
    {
      type: 'item',
      text: this.$tc('navigation_menu.accounts_balances'),
      route: '/accounts-balances',
      class: 'accounts-balances',
      icon: 'mdi-briefcase-variant'
    },
    {
      type: 'group',
      text: this.$tc('navigation_menu.history'),
      route: '/history',
      class: 'history',
      icon: 'mdi-history',
      items: [
        {
          type: 'item',
          text: this.$tc('navigation_menu.history_sub.trades'),
          route: '/history/trades',
          icon: 'mdi-shuffle-variant',
          class: 'history-trades'
        },
        {
          type: 'item',
          text: this.$tc('navigation_menu.history_sub.deposits_withdrawals'),
          route: '/history/deposits-withdrawals',
          icon: 'mdi-bank-transfer',
          class: 'deposits-withdrawals'
        },
        {
          type: 'item',
          text: this.$tc('navigation_menu.history_sub.ethereum_transactions'),
          route: '/history/transactions',
          icon: 'mdi-swap-horizontal-bold',
          class: 'eth-transactions'
        }
      ]
    },
    {
      type: 'group',
      text: this.$tc('navigation_menu.defi'),
      route: Routes.DEFI_OVERVIEW,
      icon: 'mdi-finance',
      class: 'defi',
      items: [
        {
          type: 'item',
          text: this.$tc('navigation_menu.defi_sub.overview'),
          route: Routes.DEFI_OVERVIEW,
          icon: 'mdi-chart-box',
          class: 'defi-overview'
        },
        {
          type: 'item',
          text: this.$tc('navigation_menu.defi_sub.deposits'),
          route: Routes.DEFI_DEPOSITS,
          icon: 'mdi-bank-transfer-in',
          class: 'defi-deposits'
        },
        {
          type: 'item',
          text: this.$tc('navigation_menu.defi_sub.liabilities'),
          route: Routes.DEFI_LIABILITIES,
          icon: 'mdi-bank-transfer-out',
          class: 'defi-liabilities'
        }
      ]
    },
    {
      type: 'item',
      text: this.$tc('navigation_menu.statistics'),
      route: '/statistics',
      class: 'statistics',
      icon: 'mdi-chart-bar'
    },
    {
      type: 'item',
      text: this.$tc('navigation_menu.staking'),
      route: Routes.STAKING,
      class: 'staking',
      icon: 'mdi-inbox-arrow-down'
    },
    {
      type: 'item',
      text: this.$tc('navigation_menu.tax_report'),
      route: '/tax-report',
      class: 'tax-report',
      icon: 'mdi-calculator'
    },
    {
      type: 'divider'
    },
    {
      type: 'item',
      text: this.$tc('navigation_menu.api_keys'),
      route: '/settings/api-keys/rotki-premium',
      class: 'settings__api-keys',
      icon: 'mdi-key-chain-variant'
    },
    {
      type: 'item',
      text: this.$tc('navigation_menu.import_data'),
      route: '/import',
      icon: 'mdi-database-import'
    }
  ];
}
</script>

<style scoped lang="scss">
.navigation-menu {
  &__item {
    &--active {
      background-color: var(--v-primary-base);
      color: white !important;
    }
  }
}

::v-deep {
  .v-list-group {
    &__header {
      &__append-icon {
        margin-left: -18px !important;
      }

      .v-list-item {
        padding-left: 0 !important;
      }
    }
  }
}
</style>
