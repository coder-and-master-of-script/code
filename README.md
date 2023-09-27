# code for minecraft nuke
nuke minecraft
package com.example.rileymod.items;

import net.minecraft.item.Food;
import net.minecraft.item.Item;
import net.minecraft.item.ItemStack;
import net.minecraft.item.UseAction;
import net.minecraft.world.World;
import net.minecraft.entity.LivingEntity;
import net.minecraft.potion.EffectInstance;
import net.minecraft.potion.Effects;
import net.minecraftforge.common.MinecraftForge;
import net.minecraftforge.event.entity.living.LivingDamageEvent;
import net.minecraftforge.eventbus.api.SubscribeEvent;
import net.minecraftforge.fml.common.Mod;
import net.minecraftforge.registries.ObjectHolder;

@Mod.EventBusSubscriber(modid = "rileymod", bus = Mod.EventBusSubscriber.Bus.MOD)
public class BuffItem extends Item {
    public BuffItem() {
        super(new Item.Properties()
            .food(new Food.Builder()
                .hunger(0) // Set hunger to 0 to make it non-edible
                .saturation(0)
                .alwaysEat()
                .build())
            .maxStackSize(1));
    }

    @Override
    public UseAction getUseAction(ItemStack stack) {
        return UseAction.DRINK;
    }

    @Override
    public ItemStack onItemUseFinish(ItemStack stack, World worldIn, LivingEntity entityLiving) {
        // Apply the health buff when the item is used
        entityLiving.addPotionEffect(new EffectInstance(Effects.ABSORPTION, 20 * 60, 9)); // 9 for 10 extra hearts (20 health points)
        
        // Create and give the player a unicorn item (10-heart buff)
        if (!worldIn.isRemote && entityLiving instanceof PlayerEntity) {
            PlayerEntity player = (PlayerEntity) entityLiving;
            player.addItemStackToInventory(new ItemStack(MyItems.UNICORN_ITEM));
        }
        
        return super.onItemUseFinish(stack, worldIn, entityLiving);
    }
}
