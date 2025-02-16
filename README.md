// ==UserScript==
// @name         Fake Date Override (Chrome Fix)
// @namespace    fake-date-script
// @version      1.1
// @description  Override the browser's date globally using Tampermonkey (Chrome Compatible)
// @match        *://*/*
// @grant        none
// @run-at       document-start
// ==/UserScript==

(function() {
    'use strict';

    // Define the fake date (February 16, 2025, at 00:39 UTC)
    const fakeDate = new Date('2025-02-16T00:39:00.000Z');

    // Override the Date constructor globally
    (function() {
        const OriginalDate = Date;

        function FakeDate(...args) {
            if (args.length === 0) {
                return new OriginalDate(fakeDate);
            }
            return new OriginalDate(...args);
        }

        FakeDate.prototype = OriginalDate.prototype;
        FakeDate.now = () => fakeDate.getTime();
        FakeDate.parse = OriginalDate.parse;
        FakeDate.UTC = OriginalDate.UTC;

        Object.defineProperty(window, 'Date', {
            value: FakeDate,
            writable: false,
            configurable: false
        });
    })();
})();
