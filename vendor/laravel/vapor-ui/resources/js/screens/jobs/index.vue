<template>
    <search>
        <template slot="filters" slot-scope="{ filters, loadEntries }">
            <div class="mt-6 flex space-x-3 md:mt-0 md:ml-4">
                <div>
                    <label for="queue-input" class="block text-sm font-medium leading-5 text-gray-700">
                        Queue name
                    </label>
                    <select
                        id="queue-input"
                        v-model="filters.queue"
                        v-on:change="loadEntries"
                        class="mt-1 form-select block w-full pl-3 pr-10 py-2 text-base leading-6 border-gray-300 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 sm:text-sm sm:leading-5"
                    >
                        <option v-for="(label, value) in queues()" :value="value">{{ label }}</option>
                    </select>
                </div>
            </div>
        </template>
        <template slot="troubleshooting">
            <p>It looks like there was an error. Please check your application logs.</p>

            <p class="mt-2">
                This error may occur if you haven't properly configured "failed jobs" in your application.
            </p>
        </template>

        <template slot="row" slot-scope="{ entry }">
            <td class="max-w-0 w-full px-6 py-4 whitespace-no-wrap text-sm leading-5 text-cool-gray-900">
                <div class="ml-4">
                    <div class="text-sm leading-5 font-medium text-gray-900 truncate">
                        <p class="truncate">
                            {{ entry.message }}
                        </p>
                    </div>
                    <div v-if="entry.exception" class="text-sm leading-5 text-gray-500 truncate">
                        {{ entry.exception }}
                    </div>
                </div>
            </td>

            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-200">
                <span
                    :class="`px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-${jobColor(
                        entry.content.payload.displayName
                    )}-100 text-${jobColor(entry.content.payload.displayName)}-800`"
                >
                    {{ entry.content.payload.displayName }}
                </span>
            </td>

            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-200 text-sm leading-5 text-gray-500">
                {{ moment().utc(entry.timestamp, 'x').local().format('YYYY-MM-DD LTS') }}
            </td>
            <td class="px-6 py-4 whitespace-no-wrap text-right border-b border-gray-200 text-sm leading-5 font-medium">
                <div class="relative flex justify-end items-center">
                    <router-link
                        :to="{
                            name: `jobs-show`,
                            params: { id: entry.id, group: entry.group },
                            query: entry.filters,
                        }"
                        tag="button"
                        href="#"
                        class="w-8 h-8 inline-flex items-center justify-center text-gray-400 rounded-full bg-transparent hover:text-gray-500 focus:outline-none focus:text-gray-500 focus:bg-gray-100 transition ease-in-out duration-150"
                    >
                        <icon-eye
                            size="5"
                            class="mr-3 text-gray-400 group-hover:text-gray-500 group-focus:text-gray-500"
                        />
                    </router-link>
                </div>
            </td>
        </template>
    </search>
</template>

<script>
import JobMixin from './../../mixins/job';

export default {
    /**
     * The component's mixins.
     */
    mixins: [JobMixin],
};
</script>
