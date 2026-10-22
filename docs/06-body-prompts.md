# Positive and Negative Prompts by Body Part
## Practical reference for WAI Illustrious SDXL v1.7.0 in Forge

This chapter is a practical prompt reference, not a list of guaranteed fixes. Tags are learned visual cues; their effect depends on the checkpoint, composition, pose, resolution, and other prompt terms. Start with the model-card baseline, then add only the group that addresses a visible problem.

## 1. How to use this guide

- **Positive prompt** describes the feature you want to appear.
- **Negative prompt** discourages a feature or defect you do not want.
- Use positive anatomy tags when a feature is visible and important. Do not add detailed hands/feet tags to a tiny distant figure.
- Add negative terms only for recurring defects. Do not paste every section into every image.
- Change one group at a time, keep the seed and settings fixed, and compare results.
- If a limb is hidden or outside the frame, the model may not draw it. Use composition tags such as `full body`, `upper body`, `close-up`, or `hands visible` when appropriate.
- Negative prompts are not a substitute for inpainting, pose control, or a better source composition.

## 2. WAI baseline

The WAI Illustrious SDXL v1.7.0 model card lists this short negative baseline:

```text
bad quality, worst quality, worst detail, sketch, censor
```

A simple quality positive block often used with this model is:

```text
masterpiece, best quality, amazing quality
```

These are model-specific starting points, not mandatory tokens. For anatomy problems, append a small targeted group rather than replacing the baseline with a huge negative list.

## 3. Head and face

### Overall face
**Positive options**
```text
detailed face, delicate facial features, balanced facial features, expressive face
```
**Negative options, only if needed**
```text
bad face, deformed face, distorted face, poorly drawn face, asymmetrical face
```
Notes: `detailed face` is broad; the more concrete tags below may be more useful for a specific issue.

### Eyes
**Positive options**
```text
detailed eyes, beautiful detailed eyes, expressive eyes, symmetrical eyes
```
**Negative options**
```text
bad eyes, asymmetrical eyes, cross-eyed, malformed eyes, extra eyes
```
Notes: `symmetrical eyes` may make expressions look less natural in some poses. Do not use it automatically if you want a dramatic or uneven expression.

### Pupils, iris and gaze
**Positive options**
```text
detailed iris, visible pupils, looking at viewer, looking away, focused gaze
```
**Negative options**
```text
misaligned pupils, uneven pupils, blank eyes
```
Use only the gaze direction you want. Do not put contradictory gaze directions together.

### Eyebrows
**Positive options**
```text
detailed eyebrows, expressive eyebrows
```
**Negative options**
```text
uneven eyebrows, malformed eyebrows
```
Often unnecessary unless eyebrows are prominent in the shot.

### Nose
**Positive options**
```text
small nose, delicate nose, defined nose
```
**Negative options**
```text
distorted nose, malformed nose
```
In anime art, the nose may intentionally be minimal. `defined nose` can push the image toward more realistic rendering.

### Mouth, lips and teeth
**Positive options**
```text
detailed mouth, natural smile, closed mouth, parted lips
```
**Negative options**
```text
bad mouth, distorted mouth, malformed teeth, extra teeth
```
Pick the intended mouth state. Avoid adding `smile`, `open mouth`, and `closed mouth` together unless you are testing alternatives.

### Ears
**Positive options**
```text
detailed ears, visible ears
```
**Negative options**
```text
bad ears, malformed ears, extra ears
```
Use `visible ears` only when hair or framing should not cover them.

### Hairline, hairstyle and hair strands
**Positive options**
```text
detailed hair, detailed hair strands, layered hair, flowing hair, hair highlights
```
**Negative options**
```text
messy hair, tangled hair, poorly drawn hair
```
These negatives are style-dependent: do not use `messy hair` if you want a windswept or deliberately messy hairstyle.

## 4. Neck, shoulders and torso

### Neck
**Positive options**
```text
natural neck, slender neck
```
**Negative options**
```text
long neck, deformed neck, twisted neck
```
`slender neck` is a body-design choice, not a universal anatomy fix.

### Shoulders and collarbones
**Positive options**
```text
natural shoulders, defined collarbones, relaxed shoulders
```
**Negative options**
```text
uneven shoulders, deformed shoulders, malformed collarbones
```
Asymmetry can be correct in a turned or leaning pose; inspect the pose before adding a negative.

### Chest and ribcage
**Positive options**
```text
natural torso, balanced proportions, defined torso
```
**Negative options**
```text
bad anatomy, deformed torso, unnatural proportions, twisted torso
```
Choose shape-related descriptors only when you deliberately want a particular body type. Avoid stacking contradictory body-shape tags.

### Waist, abdomen and back
**Positive options**
```text
natural waist, defined waist, natural abdomen, natural back
```
**Negative options**
```text
deformed torso, twisted torso, unnatural waist, broken back
```
`defined waist` changes the design; it does not simply improve anatomy.

## 5. Arms, elbows, wrists and hands

### Arms
**Positive options**
```text
natural arms, well-defined arms, arms in natural position
```
**Negative options**
```text
bad arms, deformed arms, extra arms, missing arms, disconnected limbs, extra limbs
```
If the pose is complex, describe the action plainly: `arms crossed`, `one arm raised`, or `hands behind back`.

### Elbows and forearms
**Positive options**
```text
natural arm proportions, natural elbow position
```
**Negative options**
```text
deformed arms, twisted arms, unnatural arm proportions
```
These are broad terms and may have limited effect; use pose control or inpainting for stubborn errors.

### Wrists
**Positive options**
```text
natural wrists, slender wrists
```
**Negative options**
```text
deformed wrists, twisted wrists, broken wrists
```

### Hands — general
**Positive options**
```text
detailed hands, well-drawn hands, natural hands
```
**Negative options**
```text
bad hands, poorly drawn hands, deformed hands, mutated hands
```
Use the positive tags when hands are visible and large enough to matter. They do not guarantee perfect anatomy.

### Fingers and thumbs
**Positive options**
```text
five fingers, separated fingers, natural fingers, visible thumb
```
**Negative options**
```text
extra fingers, missing fingers, fused fingers, malformed fingers, extra digit, fewer digits, too many fingers
```
`five fingers` can help describe the intended anatomy but is not a guarantee. For a hand holding an object, also describe the grip and object clearly.

### Hand pose and contact
**Positive options**
```text
open hand, relaxed hands, hand on hip, holding object, hands clasped
```
**Negative options**
```text
floating hands, disconnected hands, fused hands, malformed hands
```
Use the actual pose you want. If the hand must hold a specific item, name the item and the interaction, e.g. `right hand holding a cup`.

## 6. Pelvis, hips and legs

### Pelvis and hips
**Positive options**
```text
natural hips, balanced proportions, natural pelvis
```
**Negative options**
```text
deformed hips, unnatural proportions, twisted pelvis
```

### Thighs and knees
**Positive options**
```text
natural leg proportions, natural knees, defined thighs
```
**Negative options**
```text
deformed thighs, fused thighs, extra thighs, malformed knees, twisted legs
```
Do not use `defined thighs` unless that shape is part of the desired design.

### Lower legs, ankles and legs overall
**Positive options**
```text
natural legs, well-defined legs, balanced leg proportions
```
**Negative options**
```text
bad legs, deformed legs, extra legs, missing legs, fused legs, disconnected limbs
```
For a full-body image, explicitly describe the pose and framing. Negative tags cannot reliably reconstruct a leg hidden behind another leg.

### Feet and toes
**Positive options**
```text
detailed feet, natural feet, visible toes
```
**Negative options**
```text
bad feet, poorly drawn feet, deformed feet, extra toes, missing toes, fused toes, malformed feet
```
If shoes are meant to cover the feet, describe the shoes instead of asking for detailed toes.

## 7. Overall anatomy, proportions and pose

**Positive options**
```text
good anatomy, balanced proportions, natural pose, anatomically plausible pose
```

**Negative options**
```text
bad anatomy, bad proportions, deformed, disfigured, mutated, extra limbs, missing limbs, disconnected limbs, floating limbs
```

These broad negatives are useful as a troubleshooting group, but are not all required at once. Terms such as `good anatomy` and `natural pose` are broad cues, not technical constraints.

### Body count and duplicated parts
**Negative options**
```text
duplicate, cloned body, extra head, extra arms, extra legs, extra hands, extra fingers, extra eyes
```
Use only if duplicate parts actually appear. For multiple people, do not add a blanket negative that conflicts with the intended number of subjects.

## 8. Universal copy-and-paste blocks

### A. Short WAI baseline
Positive:
```text
masterpiece, best quality, amazing quality
```
Negative:
```text
bad quality, worst quality, worst detail, sketch, censor
```

### B. General character — optional anatomy cleanup
Append to the WAI negative baseline only if anatomy errors occur:
```text
bad anatomy, bad proportions, deformed, extra limbs, missing limbs, disconnected limbs
```

### C. Hands-only cleanup
```text
bad hands, deformed hands, mutated hands, extra fingers, missing fingers, fused fingers, malformed fingers
```

### D. Face-only cleanup
```text
bad face, deformed face, distorted face, asymmetrical eyes, cross-eyed, malformed mouth
```

### E. Legs and feet cleanup
```text
bad legs, deformed legs, fused legs, extra legs, bad feet, deformed feet, extra toes, fused toes
```

Do not automatically combine B, C, D, and E. Start with the group that matches the observed defect.

## 9. A practical troubleshooting table

| Visible problem | First test in negative prompt | Positive or composition cue to test |
|---|---|---|
| Too many fingers | `extra fingers, too many fingers` | `five fingers, separated fingers` |
| Fingers merged together | `fused fingers, malformed fingers` | `separated fingers, detailed hands` |
| Hands look distorted | `bad hands, deformed hands` | Describe the hand pose simply |
| Eyes misaligned | `asymmetrical eyes, cross-eyed` | `detailed eyes` |
| Face distorted | `bad face, distorted face` | `detailed face` |
| Extra limbs | `extra limbs, extra arms, extra legs` | State the pose and number of people clearly |
| Legs merged | `fused legs, deformed legs` | Describe which leg is forward/behind |
| Feet malformed | `bad feet, malformed feet` | `detailed feet` if bare feet are visible |
| Neck too long | `long neck` | Avoid contradictory body proportions |
| Body cropped unintentionally | Test `cropped, out of frame` cautiously | `full body, feet visible` and allow enough canvas space |

The prompt cannot always fix composition by itself. If the defect persists, use inpainting for the affected region, a pose guide, or regenerate with a clearer composition.

## 10. Safe testing procedure in Forge

1. Keep checkpoint, sampler, steps, CFG, image size, and seed fixed.
2. Generate a baseline without the targeted anatomy terms.
3. Add only one small negative group.
4. Compare the same seed and inspect the same body part.
5. If it improves, keep the group; if the image gets worse, remove it.
6. For hands, face, or feet occupying a small part of the image, consider inpainting rather than adding dozens of tags.

## 11. Important caveats

- There is no single official universal positive/negative list for every body part.
- The WAI model card provides a short quality-oriented negative baseline; the anatomy groups here are practical suggestions compiled from common prompting practice.
- Tags can have side effects. For example, `bad hands` may influence the overall hand rendering, while `cropped` can affect framing.
- Do not put desired traits in the negative prompt. If you want anime art, do not negate `anime`; if you want visible bare feet, do not negate `feet`.
- Negative prompts cannot guarantee correct anatomy. Inpainting, pose conditioning, and clear framing can be more effective for persistent errors.

## Sources

- WAI Illustrious SDXL v1.7.0 model card: https://huggingface.co/LyliaEngine/waiIllustriousSDXL_v170
- Illustrious / NoobAI / Pony prompting reference: https://github.com/tsuseki/vrfu-ai/blob/main/docs/prompting.md
- InvokeAI prompting guide: https://invoke.ai/concepts/prompting-guide/
- SD.Next prompting documentation: https://vladmandic.github.io/sdnext-docs/Prompting/
