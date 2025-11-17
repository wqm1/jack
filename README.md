# jack
1111
! Sparring results - Master
if interaction_city = 1296:
	$place = 'colosseum'
	$return_lock = 'shop_general'
	$picture_type = 'pic_png'
	$bg_path = 'scene/arena_end'
	g = rand(1, picMax['arena_end'])
	if g > 1: $bg_path += '_' + $str(g) 
	if $итог_боя = 'бегство' or $итог_боя = 'вырубился' or $итог_боя = 'убит':
		fighting_experience = iif((master_fighter - boost_up['master_fighter']) < 1, 5, 0) &! defeat gives flat 2 xp up to D- and cannot raise skill higher - ImperatorAugustus
		$text[1] = '  你败倒在地上面, 当然是躺着的.' + iif(fighting_experience = 0, '不幸的是, 遭受这样的失败无法磨练你的战斗能力, 也无法提高你的耐力.', '不幸的是, 遭受这样的失败并不能提高你的耐力, 尽管它仍然可以教会你一些东西.')
		master_fighter_rate += (skill_adv_mul * fighting_experience)
		master_mood_rate -= 5
	else
	  fight_dengji = min(4, colosseum_fight)
		fighting_experience = iif((master_fighter - boost_up['master_fighter']) < 4, max(0, fight_dengji - (master_fighter - boost_up['master_fighter']))*5, 0) &! sparring cannot raise level above A+, xp gain is proportional to opponent level and zero for tiers equal or below current skill (colosseum_fight indicates tier of opponent, ranges from 1 to 5) - ImperatorAugustus
		$text[1] = '  你的最后一击将对手击倒在地.' + iif(fighting_experience = 0, '不幸的是, 击败这样的对手无法磨练你的战斗力,<<iif(colosseum_fight >= master_str, "尽管仍然可以", "也不会")>>提高你的耐力.', iif(colosseum_fight < master_str, '不幸的是, 继续击败这样的对手, 虽然仍然可以磨练战斗力, 但无法提高你的耐力.', ''))
		master_fighter_rate += min((skill_adv_mul * fighting_experience)*(max(1, fighting_experience - (master_fighter - boost_up['master_fighter']))), skill_adv_mul * 15)
		if colosseum_fight >= master_str: master_str_rate += (skill_adv_mul * 50) &! sparring the second-highest-tier opponents (4) can raise master strength up to S+, but only the top-tier opponents can raise fighting skill to A+
		if colosseum_fight - 2 > master_domination: master_domination_rate += 5 &! sparring the highest-tier opponents (5) can raise master domination up to B+ (3)
		master_mood_rate += 5
		if master_moodlet['pos_master_winner'] < 2: master_moodlet['pos_master_winner'] += 1 &! at least two wins on same day will carry over moodlet to next day -ImperatorAugustus
	end
	sparks -= 3
	master_hygiene_rate += 15
	combatant = 0 &! set master as combatant for next battle
	colosseum_fight = 0 &! clear spar type global
	fighting_experience = 0 &! used here as a local variable
	get_weapon = 0 &! stop intercepting ESC in menu_form
	$итог_боя = '' &! reset outcome so next battle does not prematurely end
	$fight_return = '' &! clear post-battle $return_lock variable
	gs '$newloc' &! update stats after combat
end
