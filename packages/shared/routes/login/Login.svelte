<script lang="typescript">
    import { FireflyEvent, loginRoute, LoginRouter, LoginRoute } from '@core/router'
    import { Transition } from 'shared/components'
    import { activeProfileId, clearActiveProfile, profiles } from 'shared/lib/profile'
    import { Locale } from '@core/i18n'
    import { onMount } from 'svelte'
    import { get } from 'svelte/store'
    import { EnterPin, SelectProfile } from './views/'

    export let locale: Locale

    let loginRouter: LoginRouter

    onMount(() => {
        loginRouter = new LoginRouter()
        if (get(activeProfileId) && get(profiles)?.find((p) => p.id === get(activeProfileId))) {
            loginRouter.next()
        } else {
            clearActiveProfile()
        }
    })

    const next = (event: CustomEvent<FireflyEvent>): void => loginRouter.next(event.detail)
    const previous = (): void => loginRouter.previous()
</script>

{#if $loginRoute === LoginRoute.Init}
    <Transition>
        <SelectProfile on:next={next} on:previous={previous} />
    </Transition>
{:else if $loginRoute === LoginRoute.EnterPin}
    <Transition>
        <EnterPin on:next={next} on:previous={previous} {locale} />
    </Transition>
{/if}
