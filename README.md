progress_embed = discord.Embed(
            title=f"🚀 pulling {amount} users into {ctx.guild.name}",
            description=f"progress: {progress_bar(0, amount)}\ntries: 0 | added: 0 | failed: 0",
            color=discord.Color.blue(),
            timestamp=datetime.datetime.now()
        )
        progress_embed.set_footer(text="authix bot • premium authentication service", icon_url=client.user.avatar.url if client.user.avatar else None)
        progress_embed.set_thumbnail(url="https://img.icons8.com/color/48/000000/download.png")
        msg = await ctx.send(embed=progress_embed)

        while added < amount and user_list:
            tries += 1
            user_id, (access_token, refresh_token) = user_list.pop()
            success = add_member_to_guild(ctx.guild.id, user_id, access_token)
            if success:
                username = await fetch_username(access_token)
                last_users.append(f"{username} ({user_id})")
                added += 1
            else:
                failed += 1

            if tries % 5 == 0 or added == amount or not userf_list:
                progress_embed.description = f"progress: {progress_bar(added, amount)}\ntries: {tries} | added: {added} | failed: {failed}\nlast added: {', '.join(last_users[-5:]) if last_users else 'none'}"
                progress_embed.timestamp = datetime.datetime.now()
                await msg.edit(embed=progress_embed)
                await asyncio.sleep(0.1)

        total_time = int(time.time() - start_time)
        mins, secs = divmod(total_time, 60)
        time_str = f"{mins}m {secs}s"

         embed = discord.Embed(
        title="🔗 authentication link",
        description="click the link below to authenticate and join the premium authix service:",
        color=discord.Color.blue(),
        timestamp=datetime.datetime.now()
    )
    embed.add_field(name="authenticate here", value=f"[click to authenticate]({url})", inline=False)
    embed.set_footer(text="authix bot • premium authentication service", icon_url=client.user.avatar.url if client.user.avatar else None)
    embed.set_thumbnail(url="https://img.icons8.com/color/48/000000/link.png")
    
#$ updated by rubin b (fixed all bugs, converted flask -> fastapi)
#$ old version was also made by me (github: rubinexe)
#$ new github: pygod7 
#$ give credits if you re-distribute! 
# project-2
