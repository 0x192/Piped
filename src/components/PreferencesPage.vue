<template>
    <div class="flex">
        <button @click="$router.go(-1) || $router.push('/')">
            <font-awesome-icon icon="chevron-left" /><span class="ml-1.5" v-t="'actions.back'" />
        </button>
    </div>
    <h1 v-t="'titles.preferences'" class="font-bold text-center" />
    <hr />
    <label for="ddlTheme" class="pref">
        <strong v-t="'actions.theme'" />
        <select id="ddlTheme" v-model="selectedTheme" class="select w-auto" @change="onChange($event)">
            <option v-t="'actions.auto'" value="auto" />
            <option v-t="'actions.dark'" value="dark" />
            <option v-t="'actions.light'" value="light" />
        </select>
    </label>
    <label class="pref" for="ddlLanguageSelection">
        <strong v-t="'actions.language_selection'" />
        <select id="ddlLanguageSelection" v-model="selectedLanguage" class="select w-auto" @change="onChange($event)">
            <option v-for="language in languages" :key="language.code" :value="language.code" v-text="language.name" />
        </select>
    </label>
    <label class="pref" for="ddlCountrySelection">
        <strong v-t="'actions.country_selection'" />
        <select id="ddlCountrySelection" v-model="countrySelected" class="select w-50" @change="onChange($event)">
            <option v-for="country in countryMap" :key="country.code" :value="country.code" v-text="country.name" />
        </select>
    </label>
    <label class="pref" for="ddlDefaultHomepage">
        <strong v-t="'actions.default_homepage'" />
        <select id="ddlDefaultHomepage" v-model="defaultHomepage" class="select w-auto" @change="onChange($event)">
            <option v-t="'titles.trending'" value="trending" />
            <option v-t="'titles.feed'" value="feed" />
        </select>
    </label>

    <h2 class="text-center" v-t="'titles.player'" />
    <label class="pref" for="chkAutoPlayVideo">
        <strong v-t="'actions.autoplay_video'" />
        <input
            id="chkAutoPlayVideo"
            v-model="autoPlayVideo"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkAudioOnly">
        <strong v-t="'actions.audio_only'" />
        <input id="chkAudioOnly" v-model="listen" class="checkbox" type="checkbox" @change="onChange($event)" />
    </label>
    <label class="pref" for="ddlDefaultQuality">
        <strong v-t="'actions.default_quality'" />
        <select id="ddlDefaultQuality" v-model="defaultQuality" class="select w-auto" @change="onChange($event)">
            <option v-t="'actions.auto'" value="0" />
            <option v-for="resolution in resolutions" :key="resolution" :value="resolution" v-text="`${resolution}p`" />
        </select>
    </label>
    <label class="pref" for="txtBufferingGoal">
        <strong v-t="'actions.buffering_goal'" />
        <input
            id="txtBufferingGoal"
            v-model="bufferingGoal"
            class="input w-24"
            type="text"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkShowComments">
        <strong v-t="'actions.show_comments'" />
        <input
            id="chkShowComments"
            v-model="showComments"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkMinimizeDescription">
        <strong v-t="'actions.minimize_description_default'" />
        <input
            id="chkMinimizeDescription"
            v-model="minimizeDescription"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkMinimizeRecommendations">
        <strong v-t="'actions.minimize_recommendations_default'" />
        <input
            id="chkMinimizeRecommendations"
            v-model="minimizeRecommendations"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkStoreWatchHistory">
        <strong v-t="'actions.store_watch_history'" />
        <input
            id="chkStoreWatchHistory"
            v-model="watchHistory"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="ddlEnabledCodecs">
        <strong v-t="'actions.enabled_codecs'" />
        <select
            id="ddlEnabledCodecs"
            v-model="enabledCodecs"
            class="select w-auto h-auto"
            multiple
            @change="onChange($event)"
        >
            <option value="av1">AV1</option>
            <option value="vp9">VP9</option>
            <option value="avc">AVC (h.264)</option>
        </select>
    </label>
    <label class="pref" for="chkDisableLBRY">
        <strong v-t="'actions.disable_lbry'" />
        <input id="chkDisableLBRY" v-model="disableLBRY" class="checkbox" type="checkbox" @change="onChange($event)" />
    </label>
    <label class="pref" for="chkEnableLBRYProxy">
        <strong v-t="'actions.enable_lbry_proxy'" />
        <input
            id="chkEnableLBRYProxy"
            v-model="proxyLBRY"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>

    <h2 class="text-center">SponsorBlock</h2>
    <p class="text-center">
        <span v-t="'actions.uses_api_from'" /><a class="link" href="https://sponsor.ajay.app/">sponsor.ajay.app</a>
    </p>
    <label class="pref" for="chkEnableSponsorblock">
        <strong v-t="'actions.enable_sponsorblock'" />
        <input
            id="chkEnableSponsorblock"
            v-model="sponsorBlock"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkSkipSponsors">
        <strong v-t="'actions.skip_sponsors'" />
        <input id="chkSkipSponsors" v-model="skipSponsor" class="checkbox" type="checkbox" @change="onChange($event)" />
    </label>
    <label class="pref" for="chkSkipIntro">
        <strong v-t="'actions.skip_intro'" />
        <input id="chkSkipIntro" v-model="skipIntro" class="checkbox" type="checkbox" @change="onChange($event)" />
    </label>
    <label class="pref" for="chkSkipOutro">
        <strong v-t="'actions.skip_outro'" />
        <input id="chkSkipOutro" v-model="skipOutro" class="checkbox" type="checkbox" @change="onChange($event)" />
    </label>
    <label class="pref" for="chkSkipPreview">
        <strong v-t="'actions.skip_preview'" />
        <input id="chkSkipPreview" v-model="skipPreview" class="checkbox" type="checkbox" @change="onChange($event)" />
    </label>
    <label class="pref" for="chkSkipInteraction">
        <strong v-t="'actions.skip_interaction'" />
        <input
            id="chkSkipInteraction"
            v-model="skipInteraction"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkSkipSelfPromo">
        <strong v-t="'actions.skip_self_promo'" />
        <input
            id="chkSkipSelfPromo"
            v-model="skipSelfPromo"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkSkipNonMusic">
        <strong v-t="'actions.skip_non_music'" />
        <input
            id="chkSkipNonMusic"
            v-model="skipMusicOffTopic"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkSkipHighlight">
        <strong v-t="'actions.skip_highlight'" />
        <input
            id="chkSkipHighlight"
            v-model="skipHighlight"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <label class="pref" for="chkSkipFiller">
        <strong v-t="'actions.skip_filler_tangent'" />
        <input id="chkSkipFiller" v-model="skipFiller" class="checkbox" type="checkbox" @change="onChange($event)" />
    </label>
    <label class="pref" for="chkShowMarkers">
        <strong v-t="'actions.show_markers'" />
        <input id="chkShowMarkers" v-model="showMarkers" class="checkbox" type="checkbox" @change="onChange($event)" />
    </label>

    <h2 class="text-center" v-t="'titles.instance'" />
    <label class="pref" for="ddlInstanceSelection">
        <strong v-text="`${$t('actions.instance_selection')}:`" />
        <select id="ddlInstanceSelection" v-model="selectedInstance" class="select w-auto" @change="onChange($event)">
            <option
                v-for="instance in instances"
                :key="instance.name"
                :value="instance.api_url"
                v-text="instance.name"
            />
        </select>
    </label>
    <label class="pref" for="chkAuthInstance">
        <strong v-text="`${$t('actions.different_auth_instance')}:`" />
        <input
            id="chkAuthInstance"
            v-model="authInstance"
            class="checkbox"
            type="checkbox"
            @change="onChange($event)"
        />
    </label>
    <template v-if="authInstance">
        <label class="pref" for="ddlAuthInstanceSelection">
            <strong v-text="`${$t('actions.instance_auth_selection')}:`" />
            <select
                id="ddlAuthInstanceSelection"
                v-model="selectedAuthInstance"
                class="select w-auto"
                @change="onChange($event)"
            >
                <option
                    v-for="instance in instances"
                    :key="instance.name"
                    :value="instance.api_url"
                    v-text="instance.name"
                />
            </select>
        </label>
    </template>
    <br />

    <!-- options that are visible only when logged in -->
    <div v-if="this.authenticated">
        <h2 class="text-center" v-t="'titles.account'"></h2>
        <label class="pref" for="txtDeleteAccountPassword">
            <strong v-t="'actions.delete_account'" />
            <div class="flex items-center">
                <input
                    id="txtDeleteAccountPassword"
                    ref="txtDeleteAccountPassword"
                    v-model="password"
                    v-on:keyup.enter="deleteAccount"
                    :placeholder="$t('login.password')"
                    :aria-label="$t('login.password')"
                    class="input w-auto mr-2"
                    type="password"
                />
                <a class="btn w-auto" @click="deleteAccount" v-t="'actions.delete_account'" />
            </div>
        </label>
        <div class="pref">
            <a class="btn w-auto" @click="logout" v-t="'actions.logout'" />
            <a
                class="btn w-auto"
                style="margin-left: 0.5em"
                @click="invalidateSession"
                v-t="'actions.invalidate_session'"
            />
        </div>
        <br />
    </div>
    <h2 id="instancesList" v-t="'actions.instances_list'" />
    <table class="table">
        <thead>
            <tr>
                <th v-t="'preferences.instance_name'" />
                <th v-t="'preferences.instance_locations'" />
                <th v-t="'preferences.has_cdn'" />
                <th v-t="'preferences.registered_users'" />
                <th class="lt-md:hidden" v-t="'preferences.version'" />
                <th v-t="'preferences.up_to_date'" />
                <th v-t="'preferences.ssl_score'" />
            </tr>
        </thead>
        <tbody v-for="instance in instances" :key="instance.name">
            <tr>
                <td v-text="instance.name" />
                <td v-text="instance.locations" />
                <td v-text="`${instance.cdn ? '&#9989;' : '&#10060;'}`" />
                <td v-text="instance.registered" />
                <td class="lt-md:hidden" v-text="instance.version" />
                <td v-text="`${instance.up_to_date ? '&#9989;' : '&#10060;'}`" />
                <td>
                    <a :href="sslScore(instance.api_url)" target="_blank" v-t="'actions.view_ssl_score'" />
                </td>
            </tr>
        </tbody>
    </table>
    <br />
    <p v-t="'info.preferences_note'" />
    <br />
    <button class="btn" v-t="'actions.reset_preferences'" @click="resetPreferences()" />
    <button class="btn mx-4" v-t="'actions.backup_preferences'" @click="backupPreferences()" />
    <label for="fileSelector" class="btn" v-t="'actions.restore_preferences'" @click="restorePreferences()" />
    <input class="hidden" id="fileSelector" ref="fileSelector" type="file" @change="restorePreferences()" />
</template>

<script>
import CountryMap from "@/utils/CountryMaps/en.json";
export default {
    data() {
        return {
            selectedInstance: null,
            authInstance: false,
            selectedAuthInstance: null,
            instances: [],
            sponsorBlock: true,
            skipSponsor: true,
            skipIntro: false,
            skipOutro: false,
            skipPreview: false,
            skipInteraction: true,
            skipSelfPromo: true,
            skipMusicOffTopic: true,
            skipHighlight: false,
            skipFiller: false,
            showMarkers: true,
            selectedTheme: "dark",
            autoPlayVideo: true,
            listen: false,
            resolutions: [144, 240, 360, 480, 720, 1080, 1440, 2160, 4320],
            defaultQuality: 0,
            bufferingGoal: 10,
            countryMap: CountryMap,
            countrySelected: "US",
            defaultHomepage: "trending",
            showComments: true,
            minimizeDescription: false,
            minimizeRecommendations: false,
            watchHistory: false,
            selectedLanguage: "en",
            languages: [
                { code: "ar", name: "Arabic" },
                { code: "az", name: "Azərbaycan" },
                { code: "bn", name: "বাংলা" },
                { code: "bs", name: "Bosanski" },
                { code: "ca", name: "Català" },
                { code: "cs", name: "Čeština" },
                { code: "da", name: "Dansk" },
                { code: "de", name: "Deutsch" },
                { code: "el", name: "Ελληνικά" },
                { code: "es", name: "Español" },
                { code: "en", name: "English" },
                { code: "eo", name: "Esperanto" },
                { code: "et", name: "Eesti" },
                { code: "fa", name: "فارسی" },
                { code: "fi", name: "Suomi" },
                { code: "fr", name: "Français" },
                { code: "he", name: "עברית" },
                { code: "hi", name: "हिंदी" },
                { code: "id", name: "Indonesia" },
                { code: "is", name: "Íslenska" },
                { code: "hr", name: "Hrvatski" },
                { code: "it", name: "Italiano" },
                { code: "ja", name: "日本語" },
                { code: "ko", name: "한국어" },
                { code: "lt", name: "Lietuvių kalba" },
                { code: "ml", name: "മലയാളം" },
                { code: "nb_NO", name: "Norwegian Bokmål" },
                { code: "nl", name: "Nederlands" },
                { code: "pl", name: "Polski" },
                { code: "pt", name: "Português" },
                { code: "pt_PT", name: "Português (Portugal)" },
                { code: "pt_BR", name: "Português (Brasil)" },
                { code: "ru", name: "Русский" },
                { code: "sr", name: "Српски" },
                { code: "sv", name: "Svenska" },
                { code: "ta", name: "தமிழ்" },
                { code: "th", name: "ไทย" },
                { code: "tr", name: "Türkçe" },
                { code: "uk", name: "Українська" },
                { code: "vi", name: "Tiếng Việt" },
                { code: "zh_Hant", name: "繁體中文" },
                { code: "zh_Hans", name: "简体中文" },
            ],
            enabledCodecs: ["vp9", "avc"],
            disableLBRY: false,
            proxyLBRY: false,
            password: null,
        };
    },
    activated() {
        document.title = this.$t("titles.preferences") + " - Piped";
    },
    async mounted() {
        if (Object.keys(this.$route.query).length > 0) this.$router.replace({ query: {} });

        this.fetchJson("https://piped-instances.kavin.rocks/").then(resp => {
            this.instances = resp;
            if (this.instances.filter(instance => instance.api_url == this.apiUrl()).length == 0)
                this.instances.push({
                    name: "Custom Instance",
                    api_url: this.apiUrl(),
                    locations: "Unknown",
                    cdn: false,
                });
        });

        if (this.testLocalStorage) {
            this.selectedInstance = this.getPreferenceString("instance", "https://pipedapi.kavin.rocks");
            this.authInstance = this.getPreferenceBoolean("authInstance", false);
            this.selectedAuthInstance = this.getPreferenceString("auth_instance_url", this.selectedInstance);

            this.sponsorBlock = this.getPreferenceBoolean("sponsorblock", true);
            if (localStorage.getItem("selectedSkip") !== null) {
                var skipList = localStorage.getItem("selectedSkip").split(",");
                this.skipSponsor =
                    this.skipIntro =
                    this.skipOutro =
                    this.skipPreview =
                    this.skipInteraction =
                    this.skipSelfPromo =
                    this.skipMusicOffTopic =
                    this.skipHighlight =
                    this.skipFiller =
                        false;
                skipList.forEach(skip => {
                    switch (skip) {
                        case "sponsor":
                            this.skipSponsor = true;
                            break;
                        case "intro":
                            this.skipIntro = true;
                            break;
                        case "outro":
                            this.skipOutro = true;
                            break;
                        case "preview":
                            this.skipPreview = true;
                            break;
                        case "interaction":
                            this.skipInteraction = true;
                            break;
                        case "selfpromo":
                            this.skipSelfPromo = true;
                            break;
                        case "music_offtopic":
                            this.skipMusicOffTopic = true;
                            break;
                        case "poi_highlight":
                            this.skipHighlight = true;
                            break;
                        case "filler":
                            this.skipFiller = true;
                            break;
                        default:
                            console.log("Unknown sponsor type: " + skip);
                            break;
                    }
                });
            }

            this.showMarkers = this.getPreferenceBoolean("showMarkers", true);
            this.selectedTheme = this.getPreferenceString("theme", "dark");
            this.autoPlayVideo = this.getPreferenceBoolean("playerAutoPlay", true);
            this.listen = this.getPreferenceBoolean("listen", false);
            this.defaultQuality = Number(localStorage.getItem("quality"));
            this.bufferingGoal = Math.max(Number(localStorage.getItem("bufferGoal")), 10);
            this.countrySelected = this.getPreferenceString("region", "US");
            this.defaultHomepage = this.getPreferenceString("homepage", "trending");
            this.showComments = this.getPreferenceBoolean("comments", true);
            this.minimizeDescription = this.getPreferenceBoolean("minimizeDescription", false);
            this.minimizeRecommendations = this.getPreferenceBoolean("minimizeRecommendations", false);
            this.watchHistory = this.getPreferenceBoolean("watchHistory", false);
            this.selectedLanguage = this.getPreferenceString("hl", await this.defaultLangage);
            this.enabledCodecs = this.getPreferenceString("enabledCodecs", "vp9,avc").split(",");
            this.disableLBRY = this.getPreferenceBoolean("disableLBRY", false);
            this.proxyLBRY = this.getPreferenceBoolean("proxyLBRY", false);
            if (this.selectedLanguage != "en") {
                try {
                    this.CountryMap = await import(`../utils/CountryMaps/${this.selectedLanguage}.json`).then(
                        val => val.default,
                    );
                } catch (e) {
                    console.error("Countries not translated into " + this.selectedLanguage);
                }
            }
        }
    },
    methods: {
        async onChange() {
            if (this.testLocalStorage) {
                var shouldReload = false;

                if (
                    this.getPreferenceString("theme", "dark") !== this.selectedTheme ||
                    this.getPreferenceBoolean("watchHistory", false) != this.watchHistory ||
                    this.getPreferenceString("hl", await this.defaultLangage) !== this.selectedLanguage ||
                    this.getPreferenceString("enabledCodecs", "av1,vp9,avc") !== this.enabledCodecs.join(",")
                )
                    shouldReload = true;

                localStorage.setItem("instance", this.selectedInstance);
                localStorage.setItem("authInstance", this.authInstance);
                localStorage.setItem("auth_instance_url", this.selectedAuthInstance);
                localStorage.setItem("sponsorblock", this.sponsorBlock);

                var sponsorSelected = [];
                if (this.skipSponsor) sponsorSelected.push("sponsor");
                if (this.skipIntro) sponsorSelected.push("intro");
                if (this.skipOutro) sponsorSelected.push("outro");
                if (this.skipPreview) sponsorSelected.push("preview");
                if (this.skipInteraction) sponsorSelected.push("interaction");
                if (this.skipSelfPromo) sponsorSelected.push("selfpromo");
                if (this.skipMusicOffTopic) sponsorSelected.push("music_offtopic");
                if (this.skipHighlight) sponsorSelected.push("poi_highlight");
                if (this.skipFiller) sponsorSelected.push("filler");
                localStorage.setItem("selectedSkip", sponsorSelected);

                localStorage.setItem("showMarkers", this.showMarkers);
                localStorage.setItem("theme", this.selectedTheme);
                localStorage.setItem("playerAutoPlay", this.autoPlayVideo);
                localStorage.setItem("listen", this.listen);
                localStorage.setItem("quality", this.defaultQuality);
                localStorage.setItem("bufferGoal", this.bufferingGoal);
                localStorage.setItem("region", this.countrySelected);
                localStorage.setItem("homepage", this.defaultHomepage);
                localStorage.setItem("comments", this.showComments);
                localStorage.setItem("minimizeDescription", this.minimizeDescription);
                localStorage.setItem("minimizeRecommendations", this.minimizeRecommendations);
                localStorage.setItem("watchHistory", this.watchHistory);
                localStorage.setItem("hl", this.selectedLanguage);
                localStorage.setItem("enabledCodecs", this.enabledCodecs.join(","));
                localStorage.setItem("disableLBRY", this.disableLBRY);
                localStorage.setItem("proxyLBRY", this.proxyLBRY);

                if (shouldReload) window.location.reload();
            }
        },
        sslScore(url) {
            return "https://www.ssllabs.com/ssltest/analyze.html?d=" + new URL(url).host + "&latest";
        },
        async deleteAccount() {
            this.fetchJson(this.authApiUrl() + "/user/delete", null, {
                method: "POST",
                headers: {
                    Authorization: this.getAuthToken(),
                },
                body: JSON.stringify({
                    password: this.password,
                }),
            }).then(resp => {
                if (!resp.error) {
                    this.logout();
                } else alert(resp.error);
            });
        },
        logout() {
            // reset the auth token
            localStorage.removeItem("authToken" + this.hashCode(this.authApiUrl()));
            // redirect to trending page
            window.location = "/";
        },
        resetPreferences() {
            if (!confirm(this.$t("actions.confirm_reset_preferences"))) return;
            // clear the local storage
            localStorage.clear();
            // redirect to the home page
            window.location = "/";
        },
        async invalidateSession() {
            this.fetchJson(this.authApiUrl() + "/logout", null, {
                method: "POST",
                headers: {
                    Authorization: this.getAuthToken(),
                },
            }).then(resp => {
                if (!resp.error) {
                    this.logout();
                } else alert(resp.error);
            });
        },
        backupPreferences() {
            const data = JSON.stringify(localStorage);
            this.download(data, "preferences.json", "application/json");
        },
        restorePreferences() {
            var file = this.$refs.fileSelector.files[0];
            file.text().then(text => {
                const data = JSON.parse(text);
                Object.keys(data).forEach(function (key) {
                    localStorage.setItem(key, data[key]);
                });
                window.location.reload();
            });
        },
    },
};
</script>

<style>
.pref {
    @apply flex justify-between items-center my-2 mx-[15vw] lt-md:mx-[2vw];
}
</style>
