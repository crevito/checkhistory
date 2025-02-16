// ==UserScript==
// @name         Fake Date Override
// @namespace    fake-date-script
// @version      1.0
// @description  Override browser date globally using Tampermonkey
// @match        *://*/*
// @grant        unsafeWindow
// @run-at       document-start
// ==/UserScript==

(function() {
    'use strict';

    // Define the fake date: February 16, 2025, at 00:39 UTC
    const fakeDate = new Date('2025-02-16T00:39:00.000Z');

    // Override the global Date object
    class FakeDate extends Date {
        constructor(...args) {
            if (args.length === 0) {
                return fakeDate;
            }
            return new Date(...args);
        }

        static now() {
            return fakeDate.getTime();
        }

        static parse(str) {
            return Date.parse(str);
        }

        static UTC(...args) {
            return Date.UTC(...args);
        }
    }

    // Override Date in the actual page context
    unsafeWindow.Date = FakeDate;
})();
