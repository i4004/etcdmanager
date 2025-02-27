<template>
    <v-layout fluid grid-list-md>
        <v-flex xs12>
            <v-expansion-panel focusable dark class="help" v-model="help">
                <v-expansion-panel-content dark class="darker">
                    <template v-slot:actions>
                        <v-tooltip
                            data-test="health.help.tooltip"
                            slot="prepend"
                            bottom
                            max-width="200"
                        >
                            <v-icon
                                data-test="health.help.icon"
                                slot="activator"
                                color="primary"
                                light
                                medium
                            >help</v-icon>
                            <span data-test="health.help.span">{{ $t('common.help.tooltip') }}</span>
                        </v-tooltip>
                    </template>
                    <template v-slot:header>
                        <v-toolbar-title
                            data-test="health.title.toolbar-title"
                        >{{ $t("cluster.title") }}</v-toolbar-title>
                    </template>
                    <v-tabs v-model="helpbar" dark color="black" slider-color="primary" grow>
                        <v-tab
                            data-test="health.help-info.tab"
                            ripple
                        >{{ $t('common.help.tabs.info') }}</v-tab>
                        <v-tab-item>
                            <v-card dark>
                                <v-card-text>
                                    <h2
                                        data-test="health.help-info-title.h3"
                                        class="title"
                                    >{{ $t("common.help.infoTitle") }}</h2>
                                    <p data-test="health.help-spacer-1.p" class="spacer"></p>
                                    <p
                                        data-test="health.help-text.p"
                                        v-html="platformService.getHelp($t('cluster.help.text'))"
                                    ></p>
                                    <p data-test="health.help-spacer-2.p" class="spacer"></p>
                                </v-card-text>
                            </v-card>
                        </v-tab-item>
                        <v-tab
                            data-test="health.help-shortcuts.tab"
                            ripple
                        >{{ $t('common.help.tabs.shortcuts') }}</v-tab>
                        <v-tab-item>
                            <v-card dark>
                                <v-card-text>
                                    <v-layout align-center justify-start row>
                                        <v-flex xs2>
                                            <p
                                                data-test="health.help-shortcuts-rounded.p"
                                                class="rounded"
                                            >{{ `${platformService.getMeta()} + h` }}</p>
                                        </v-flex>
                                        <v-flex xs10>
                                            <p
                                                data-test="health.help-shortcuts-help.p"
                                                class="label"
                                            >{{ $t("common.help.shortcuts.help") }}</p>
                                        </v-flex>
                                    </v-layout>
                                </v-card-text>
                            </v-card>
                        </v-tab-item>
                    </v-tabs>
                    <hr class="blackLine" />
                </v-expansion-panel-content>
            </v-expansion-panel>
            <v-card>
                <v-list dense>
                    <v-list-tile>
                        <v-list-tile-content
                            data-test="health.header-clusterId.tile-content"
                        >{{ $t("cluster.header.clusterId") }}:</v-list-tile-content>
                        <v-list-tile-content
                            data-test="health.data-header-clusterId.tile-content"
                            class="align-end"
                        >{{ data.header.cluster_id }}</v-list-tile-content>
                    </v-list-tile>
                    <v-list-tile>
                        <v-list-tile-content
                            data-test="health.header-memberId.tile-content"
                        >{{ $t("cluster.header.memberId") }}:</v-list-tile-content>
                        <v-list-tile-content
                            data-test="health.data-header-memberId.tile-content"
                            class="align-end"
                        >{{ data.header.member_id }}</v-list-tile-content>
                    </v-list-tile>
                    <v-list-tile>
                        <v-list-tile-content
                            data-test="health.header-revision.tile-content"
                        >{{ $t("cluster.header.revision") }}:</v-list-tile-content>
                        <v-list-tile-content
                            data-test="health.data-header-revision.tile-content"
                            class="align-end"
                        >{{ data.header.revision }}</v-list-tile-content>
                    </v-list-tile>
                    <v-list-tile>
                        <v-list-tile-content
                            data-test="health.header-raftTerm.tile-content"
                        >{{ $t("cluster.header.raftTerm") }}:</v-list-tile-content>
                        <v-list-tile-content
                            data-test="health.data-header-raftTerm.tile-content"
                            class="align-end"
                        >{{ data.header.raft_term }}</v-list-tile-content>
                    </v-list-tile>
                </v-list>
            </v-card>
            <v-data-iterator
                :items="data.members"
                :rows-per-page-items="rowsPerPageItems"
                :pagination.sync="pagination"
                content-tag="v-layout"
                row
                wrap
            >
                <template v-slot:header>
                    <v-toolbar dark flat>
                        <v-toolbar-title
                            data-test="health.cluster-subtitle.toolbar-title"
                        >{{ $t("cluster.subtitle") }}</v-toolbar-title>
                        <v-divider
                            data-test="health.cluster-subtitle.divider"
                            class="mx-2"
                            inset
                            vertical
                        ></v-divider>
                        <v-spacer data-test="health.cluster-subtitle.spacer"></v-spacer>
                    </v-toolbar>
                </template>
                <template v-slot:item="props">
                    <v-flex xs12 sm6 md4 lg3>
                        <v-card>
                            <v-card-title
                                data-test="health.cluster-propName.card-title"
                                class="subheading font-weight-bold"
                            >{{ props.item.name }}</v-card-title>
                            <v-divider data-test="health.cluster-propName.divider"></v-divider>
                            <v-list dense>
                                <v-list-tile>
                                    <v-list-tile-content
                                        data-test="health.cluster-columns-id.tile-content"
                                    >{{ $t("cluster.columns.id") }}:</v-list-tile-content>
                                    <v-list-tile-content
                                        data-test="health.cluster-propItem-id.tile-content"
                                        class="align-end"
                                    >{{ props.item.ID }}</v-list-tile-content>
                                </v-list-tile>

                                <v-list-tile>
                                    <v-list-tile-content
                                        data-test="health.cluster-columns-clientUrl.tile-content"
                                    >{{ $t("cluster.columns.clientUrls") }}:</v-list-tile-content>
                                    <v-list-tile-content
                                        data-test="health.cluster-propItem-clientUrl.tile-content"
                                        class="align-end"
                                    >{{ props.item.clientURLs.join(',') }}</v-list-tile-content>
                                </v-list-tile>

                                <v-list-tile>
                                    <v-list-tile-content
                                        data-test="health.cluster-columns-peerUrl.tile-content"
                                    >{{ $t("cluster.columns.peerUrls") }}:</v-list-tile-content>
                                    <v-list-tile-content
                                        data-test="health.cluster-propitem-peerUrl.tile-content"
                                        class="align-end"
                                    >{{ props.item.peerURLs.join(',') }}</v-list-tile-content>
                                </v-list-tile>
                            </v-list>
                            <v-card-actions>
                                <v-toolbar dense flat color="grey darken-2">
                                    <v-tooltip bottom max-width="200">
                                        <template v-slot:activator="{ on }">
                                            <v-hover>
                                                <v-btn
                                                    data-test="health.cluster-healthCheck.button"
                                                    slot-scope="{ hover }"
                                                    :color="hover ? 'warning' : 'none'"
                                                    icon
                                                    @click="healthCheck(props.item.ID)"
                                                    v-on="on"
                                                >
                                                    <v-icon
                                                        data-test="health.cluster-favorite.icon"
                                                    >favorite</v-icon>
                                                </v-btn>
                                            </v-hover>
                                        </template>
                                        <span
                                            data-test="health.actions-check.span"
                                        >{{ $t("cluster.actions.check") }}</span>
                                    </v-tooltip>
                                    <v-tooltip bottom max-width="200">
                                        <template v-slot:activator="{ on }">
                                            <v-hover>
                                                <v-btn
                                                    data-test="health.cluster-status.button"
                                                    slot-scope="{ hover }"
                                                    :color="hover ? 'warning' : 'none'"
                                                    icon
                                                    @click="status()"
                                                    v-on="on"
                                                >
                                                    <v-icon
                                                        data-test="health.cluster-info.icon"
                                                    >info</v-icon>
                                                </v-btn>
                                            </v-hover>
                                        </template>
                                        <span
                                            data-test="health.actions-status.span"
                                        >{{ $t("cluster.actions.status") }}</span>
                                    </v-tooltip>
                                    <v-spacer></v-spacer>
                                    <v-icon
                                        data-test="health.props-item-id.icon"
                                        dark
                                        v-bind:color="getColor(props.item.ID)"
                                    >{{ getIcon(props.item.ID) }}</v-icon>
                                </v-toolbar>
                            </v-card-actions>
                        </v-card>
                    </v-flex>
                </template>
            </v-data-iterator>

            <v-dialog v-model="statusDialog" v-if="currentStats" persistent max-width="290">
                <v-card dark>
                    <v-toolbar dark flat>
                        <v-toolbar-title
                            data-test="health.cluster-dialogs-info-title.toolbar-title"
                        >{{ $t("cluster.dialogs.info.title") }}</v-toolbar-title>
                    </v-toolbar>
                    <v-list dense>
                        <v-list-tile>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-labels-db.tile-content"
                            >{{ $t("cluster.dialogs.info.labels.db") }}:</v-list-tile-content>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-currentStats-dbSize.v-list-tile-content"
                                class="align-end"
                            >{{ currentStats.dbSize }}</v-list-tile-content>
                        </v-list-tile>

                        <v-list-tile>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-labels-leader.tile-content"
                            >{{ $t("cluster.dialogs.info.labels.leader") }}:</v-list-tile-content>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-currentStats-leader.tile-content"
                                class="align-end"
                            >{{ currentStats.leader }}</v-list-tile-content>
                        </v-list-tile>

                        <v-list-tile>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-labels-raftIndex.tile-content"
                            >{{ $t("cluster.dialogs.info.labels.raftIndex") }}:</v-list-tile-content>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-currentStats-raftIndex.tile-content"
                                class="align-end"
                            >{{ currentStats.raftIndex }}</v-list-tile-content>
                        </v-list-tile>

                        <v-list-tile>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-labels-raftTerm.tile-content"
                            >{{ $t("cluster.dialogs.info.labels.raftTerm") }}:</v-list-tile-content>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-currentStats-raftTerm.tile-content"
                                class="align-end"
                            >{{ currentStats.raftTerm }}</v-list-tile-content>
                        </v-list-tile>

                        <v-list-tile>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-labels-version.tile-content"
                            >{{ $t("cluster.dialogs.info.labels.version") }}:</v-list-tile-content>
                            <v-list-tile-content
                                data-test="health.cluster-dialogs-info-currentStats-version.tile-content"
                                class="align-end"
                            >{{ currentStats.version }}</v-list-tile-content>
                        </v-list-tile>
                    </v-list>
                    <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn
                            data-test="health.cluster-dialogs-close.button"
                            color="warning"
                            round
                            @click="cancelStatusDialog"
                        >{{ $t("cluster.dialogs.info.actions.close") }}</v-btn>
                    </v-card-actions>
                </v-card>
            </v-dialog>
        </v-flex>
    </v-layout>
</template>

<script lang="ts">
import Vue from 'vue';
import Component from 'vue-class-component';
import StatsService from '../services/stats.service';
import { IMember, IAlarmResponse, IStatusResponse } from 'etcd3';
import Messages from '../lib/messages';
import { GenericObject } from '../../types';
import { PlatformService } from '../services/platform.service';
import Mousetrap, { ExtendedKeyboardEvent } from 'mousetrap';

@Component({
    name: 'health-check',
})
export default class HealthCheck extends Vue {
    private etcd: StatsService;
    public data: {
        members: IMember[];
        header: GenericObject;
    } = {
        members: [],
        header: {},
    };
    public health: {
        [key: string]: IAlarmResponse;
    } = {};

    public rowsPerPageItems = [4, 8, 12];
    public pagination = {
        rowsPerPage: 4,
    };
    public currentStats: IStatusResponse | null = null;
    public statusDialog: boolean = false;
    public help: number | null = null;
    public platformService: PlatformService;
    public helpbar: any = 0;

    constructor() {
        super();
        this.etcd = new StatsService(this.$store.state.connection.getClient());
        this.platformService = new PlatformService();
    }

    created() {
        const keyboardEvents = new Mousetrap();
        keyboardEvents.bind(['ctrl+h', 'meta+h'], (e: ExtendedKeyboardEvent) => {
            e.preventDefault();
            this.help = this.help === null ? 0 : null;
        });
        this.fetchMembers();
    }

    public getIcon(id: string) {
        if (!this.health[id]) {
            return 'remove';
        }
        return this.health[id].alarms.length ? 'cancel' : 'check_circle';
    }

    public getColor(id: string) {
        if (!this.health[id]) {
            return 'none';
        }
        return this.health[id].alarms.length ? 'error' : 'success';
    }

    async healthCheck(id: string) {
        this.health[id] = await this.etcd.getAlarms(id);
        this.fetchMembers();
    }

    async status() {
        this.currentStats = await this.etcd.getStats();
        this.statusDialog = true;
    }

    public cancelStatusDialog() {
        this.statusDialog = false;
    }

    private async fetchMembers() {
        try {
            const res = await this.etcd.listMembers();
            this.data = res;
        } catch (e) {
            this.$store.commit('message', Messages.error(e));
        }
    }
}
</script>

<style scoped>
</style>
