<template>
    <div class="content">
        <h1>Accidentally left the caps lock on and typed something, but can't be bothered to start again and retype it
            all?</h1>
        <p>Simply enter your text and choose the case you want to convert it to.</p>
        <textarea ref="textarea" name="text" id="text" cols="30" rows="10" v-model="text"
                  placeholder="Type or paste your content here"></textarea>
        <div class="actions">
            <button id="sentence" @click.prevent="action('sentence')" class="btn">Sentence case</button>
            <button id="lower" @click.prevent="action('lower')" class="btn">lower case</button>
            <button id="upper" @click.prevent="action('upper')" class="btn">UPPER CASE</button>
            <button id="capitalized" @click.prevent="action('capitalized')" class="btn">Capitalized Case</button>
            <button id="alternating" @click.prevent="action('alternating')" class="btn">aLtErNaTiNg cAsE</button>
            <button id="title" @click.prevent="action('title')" class="btn">Title Case</button>
            <button id="inverse" @click.prevent="action('inverse')" class="btn">InVeRsE CaSe</button>
            <button id="download" @click.prevent="action('download')" class="btn" style="display: inline-block;">
                Download Text
            </button>
            <button id="copy" @click.prevent="action('copy')" class="btn" data-clipboard-target="#content">Copy to
                Clipboard
            </button>
            <button id="clear" @click.prevent="action('clear')" class="btn">Clear</button>
        </div>
        <div>
            Character Count: {{ charCount }} | Word Count: {{ wordCount }}
        </div>
    </div>
</template>

<script>
    const URL = window.URL || window.webkitURL;
    export default {
        data() {
            return {
                text: '',
            };
        },
        computed: {
            charCount() {
                return this.text.length
            },
            wordCount() {
                if (this.charCount) {
                    return this.text.trim().split(/\s+/).length;
                }
                return 0;
            }
        },
        methods: {
            action(action) {
                switch (action) {
                    case 'sentence':
                        this.sentence();
                        break;
                    case 'lower':
                        this.lower();
                        break;
                    case 'upper':
                        this.upper();
                        break;
                    case 'capitalized':
                        this.capitalized();
                        break;
                    case 'alternating':
                        this.alternating();
                        break;
                    case 'title':
                        this.title();
                        break;
                    case 'inverse':
                        this.inverse();
                        break;
                    case 'download':
                        this.download();
                        break;
                    case 'copy':
                        this.copy();
                        break;
                    case 'clear':
                        this.clear();
                        break;
                }
            },
            sentence() {
                let e = this.text.toLowerCase();
                this.text = e.charAt(0).toUpperCase() + e.slice(1)
            },
            lower() {
                this.text = this.text.toLocaleLowerCase();
            },
            upper() {
                this.text = this.text.toLocaleUpperCase();

            },
            capitalized() {
                let str = this.text.toLowerCase();
                str = str.split(" ");

                for (let i = 0, x = str.length; i < x; i++) {
                    str[i] = str[i][0].toUpperCase() + str[i].substr(1);
                }

                this.text = str.join(" ");
            },
            alternating() {
                let output = '';
                let s = this.text.toLowerCase();

                for (let i = 0; i < s.length; i++) {
                    if (s[i].toUpperCase() === s[i]) {
                        output = output + s[i].toLowerCase();
                    } else if (s[i].toLowerCase() === s[i]) {
                        output = output + s[i].toUpperCase();
                    } else {
                        output = output + s[i];
                    }
                }
                this.text = output;
            },
            title() {
                let e = this.text.toLowerCase();
                e = this.s(e).replace(/\b(A|An|And|As|At|But|By|En|For|If|In|Of|On|Or|The|To|Vs?\\.?|Via)\b/g, function (e) {
                    return e.toLowerCase()
                })
                e = e.replace(/(?:([\.\?!] |\n|^))(a|an|and|as|at|but|by|en|for|if|in|of|on|or|the|to|vs?\\.?|via)/g, function (e) {
                    return this.s(e)
                })
                this.text = e;
            },
            inverse() {
                let s = '';
                let str = this.text;
                let i = 0;
                while (i < str.length) {
                    let n = str.charAt(i);
                    if (n === n.toUpperCase()) {
                        // *Call* toLowerCase
                        n = n.toLowerCase();
                    } else {
                        // *Call* toUpperCase
                        n = n.toUpperCase();
                    }

                    i += 1;
                    s += n;
                }
                this.text = s;
            },
            download() {
                let textToWrite = this.text;
                let textFileAsBlob = new Blob([textToWrite], {type:'text/plain'});
                let fileNameToSaveAs = "convertcase-net.txt";

                let downloadLink = document.createElement("a");
                downloadLink.download = fileNameToSaveAs;
                downloadLink.innerHTML = "Download File";
                if (URL)
                {
                    // Chrome allows the link to be clicked
                    // without actually adding it to the DOM.
                    downloadLink.href = URL.createObjectURL(textFileAsBlob);
                }

                downloadLink.click();

            },
            copy() {

                const el = document.createElement('textarea');
                el.value = this.text;
                document.body.appendChild(el);
                el.select();
                document.execCommand('copy');
                document.body.removeChild(el);
            },
            clear() {
                this.text = ''
            },
            i(e) {
                return e = e.replace(/\"([A-Za-z])/g, function(e) {
                    return e.toUpperCase()
                })
            },
            s(e) {
                let c = e.toLowerCase();
                c = (c + "").replace(/^(\S)|\s+(\S)/g, function (e) {
                    return e.toUpperCase()
                });
                c = this.i(c), c = c.replace(/\(([A-Za-z])/g, function (e) {
                    return e.toUpperCase()
                });
                return c;
            }

        }
    };
</script>

<style lang="scss" scoped>
    textarea {
        width: 400px;
        height: 200px;
        padding: 10px;
        border: 0;
        border-top: 3px solid #C2C2C2;
        background: #fff;
        font: inherit;
        outline: none;
        margin-bottom: 5px;

    }

    .actions {
        margin-bottom: 5px;
    }

    h1 {
        font-size: 18px;
        margin-top: 0;
    }

    .btn {
        background: #C2C2C2;
        color: #111;
        border: 0;
        padding: 5px 8px;
        border-top: 2px solid #A8A8A8;
        font-weight: 700;
        text-decoration: none;
        display: inline-block;
    }

    .actions .btn {
        border-top: 0;
        margin-bottom: 5px;
    }

    .content {
        margin: 0 auto;
        width: 100%;
        max-width: 500px;
        background: #E0E0E0;
        border-top: 3px solid #C2C2C2;
        padding: 15px 20px;
        box-sizing: border-box;
    }
</style>
