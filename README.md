# Discord-Akinator
[![NPM](https://nodei.co/npm/discord-akinator.png)](https://nodei.co/npm/discord-akinator/)

this is the [discord Akinator package](https://npmjs.com/package/discord-akinator).
<br>
with this package you can create a akinator discord bot command with the akinator api!
# EXAPMLE USAGE:

```js
const { SlashCommandBuilder, SlashCommandStringOption, ActivityType, EmbedBuilder } = require('discord.js')
const Akinator = require('discord-akinator')

module.exports = {
    data: new SlashCommandBuilder()
        .setName('akinator')
        .setDescription('Akinator Game! 🧞')
        .addStringOption(new SlashCommandStringOption()
            .setName('language')
            .setDescription('Select the language you prefer. (default: English)')
            .setChoices({
                name: 'English',
                value: 'en'
            }, {
                name: 'Arabic',
                value: 'ar'
            }, {
                name: 'Spanish',
                value: 'es'
            }, {
                name: 'French',
                value: 'fr'
            }, {
                name: 'Italian',
                value: 'it'
            }, {
                name: 'Japanese',
                value: 'jp'
            }, {
                name: 'Russian',
                value: 'ru'
            }, {
                name: 'Portuguese',
                value: 'pt'
            }, {
                name: 'Turkish',
                value: 'tr'
            }, {
                name: 'Chinese',
                value: 'cn'
            })
            .setRequired(false)
        ),
    async execute (interaction, client) {
        const language = interaction.options.getString('language', false) || 'en'
        const game = new Akinator(language)

        await game.start()
        await interaction.editReply({
            components: [game.component],
            embeds: [game.embed]
        })

        if (err instanceof Error) console.error(err)
            return await interaction.editReply({
                components: [],
                embeds: [],
                content: 'Timeout.'
            })
            await game.stop()

            try {
            const response = await msg.awaitMessageComponent({
                filter: i => ['yes', 'no'].includes(i.customId) && i.user.id === interaction.user.id,
                time: 30_000
            })

            const title = response.customId === 'yes'
                ? 'Awesome! Thanks for playing'
                : 'GG!'

            await msg.edit({
                components: [],
                embeds: [embed.setTitle(title)]
            })
        } catch {
            await msg.edit({
                components: [],
                embeds: [embed.setTitle(null)]
            }).catch(() => null)
        }
    }
}
```
<br>

# Full code: 

```js
const { SlashCommandBuilder, SlashCommandStringOption, ActivityType, EmbedBuilder } = require('discord.js')
const Akinator = require('discord-akinator')

module.exports = {
    data: new SlashCommandBuilder()
        .setName('akinator')
        .setDescription('Akinator Game! 🧞')
        .addStringOption(new SlashCommandStringOption()
            .setName('language')
            .setDescription('Select the language you prefer. (default: English)')
            .setChoices({
                name: 'English',
                value: 'en'
            }, {
                name: 'Arabic',
                value: 'ar'
            }, {
                name: 'Spanish',
                value: 'es'
            }, {
                name: 'French',
                value: 'fr'
            }, {
                name: 'Italian',
                value: 'it'
            }, {
                name: 'Japanese',
                value: 'jp'
            }, {
                name: 'Russian',
                value: 'ru'
            }, {
                name: 'Portuguese',
                value: 'pt'
            }, {
                name: 'Turkish',
                value: 'tr'
            }, {
                name: 'Chinese',
                value: 'cn'
            })
            .setRequired(false)
        ),
    async execute (interaction, client) {
        await interaction.deferReply()

        const language = interaction.options.getString('language', false) || 'en'
        const game = new Akinator(language)

        await game.start()
        await interaction.editReply({
            components: [game.component],
            embeds: [game.embed]
        })

        const filter = i => i.user.id === interaction.user.id && i.channelId === interaction.channelId
        const channel = await interaction.client.channels.fetch(interaction.channelId, { force: false })

        while (!game.ended) try {
            await game.ask(channel, filter)
            if (!game.ended) await interaction.editReply({ embeds: [game.embed], components: [game.component] })
        } catch (err) {
            if (err instanceof Error) console.error(err)
            return await interaction.editReply({
                components: [],
                embeds: [],
                content: 'Timeout.'
            })
        }
        await game.stop()

        const msg = await interaction.editReply({ components: [game.component], embeds: [game.embed] })
        const embed = new EmbedBuilder(msg.embeds[0].toJSON())

        try {
            const response = await msg.awaitMessageComponent({
                filter: i => ['yes', 'no'].includes(i.customId) && i.user.id === interaction.user.id,
                time: 30_000
            })

            const title = response.customId === 'yes'
                ? 'Awesome! Thanks for playing'
                : 'GG!'

            await msg.edit({
                components: [],
                embeds: [embed.setTitle(title)]
            })
        } catch {
            await msg.edit({
                components: [],
                embeds: [embed.setTitle(null)]
            }).catch(() => null)
        }
    }
}
```
(use this code in your own router!!)
if you have any other issues/questions you can open a ticket in my [github repository](https://github.com/fatih5252/discord-akinator) if you want!