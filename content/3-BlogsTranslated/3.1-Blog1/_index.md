---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Build character consistent storyboards using Amazon Nova in Amazon Bedrock – Part 1

The art of storyboarding stands as the cornerstone of modern content creation, weaving its essential role through filmmaking, animation, advertising, and UX design. Though traditionally, creators have relied on hand-drawn sequential illustrations to map their narratives, today’s AI foundation models (FMs) are transforming this landscape. FMs like [Amazon Nova Canvas](https://aws.amazon.com/ai/generative-ai/nova/creative/) and [Amazon Nova Reel](https://aws.amazon.com/ai/generative-ai/nova/creative/) offer capabilities in transforming text and image inputs into professional-grade visuals and short clips that promise to revolutionize preproduction workflows.

This technological leap forward, however, presents its own set of challenges. Although these models excel at generating diverse concepts rapidly—a boon for creative exploration—maintaining consistent character designs and stylistic coherence across scenes remains a significant hurdle. Even subtle modifications to prompts or model configurations can yield dramatically different visual outputs, potentially disrupting narrative continuity and creating additional work for content creators.

To address these challenges, we’ve developed this two-part series exploring practical solutions for achieving visual consistency. In Part 1, we deep dive into prompt engineering and character development pipelines, sharing tested prompt patterns that deliver reliable, consistent results with Amazon Nova Canvas and Amazon Nova Reel. [Part 2](https://aws.amazon.com/blogs/machine-learning/build-character-consistent-storyboards-using-amazon-nova-in-amazon-bedrock-part-2/) explores techniques like fine-tuning Amazon Nova Canvas to achieve exceptional visual consistency and precise character control.

<div style="display: flex; flex-wrap: wrap; gap: 8px; justify-content: center;">
  <img src="/images/3-BlogsTranslated/3.1-Blog1/image-1-1-300x169.jpg" alt="Girl running with llama">
  <img src="/images/3-BlogsTranslated/3.1-Blog1/image-2-2-300x169.jpg" alt="Girl running with llama">
  <img src="/images/3-BlogsTranslated/3.1-Blog1/image-3-300x169.jpg" alt="Girl running with llama">
</div>

### Consistent character design with Amazon Nova Canvas

The foundation of effective storyboarding begins with establishing well-defined character designs. Amazon Nova Canvas offers several powerful techniques to create and maintain character consistency throughout your visual narrative. To help you implement these techniques in your own projects, we’ve provided comprehensive code examples and resources in our [Github repository](https://github.com/aws-samples/sample-character-consistent-storyboard/tree/main/01-character-consistent-storyboarding-with-amazon-nova). We encourage you to follow along as we walk through each step in detail. If you’re new to Amazon Nova Canvas, we recommend first reviewing [Generating images with Amazon Nova](https://docs.aws.amazon.com/nova/latest/userguide/image-generation.html) to familiarize yourself with the basic concepts.

### Basic text prompting

Amazon Nova Canvas transforms text descriptions into visual representations. Unlike large language models (LLMs), image generation models don’t interpret commands or engage in reasoning—they respond best to descriptive captions. Including specific details in your prompts, such as physical attributes, clothing, and styling elements, directly influences the generated output.

For example, *“A 7-year-old Peruvian girl with dark hair in two low braids wearing a school uniform”* provides clear visual elements for the model to generate an initial character concept, as shown in the following example image.

![Girl](/images/3-BlogsTranslated/3.1-Blog1/image-4-1-300x300.jpeg)
---

### Visual style implementation

Consistency in storyboarding requires both character features and unified visual style. Our approach separates style information into two key components in the prompt:

- **Style description** – An opening phrase that defines the visual medium (for example, *“A graphic novel style illustration of”*)

- **Style details** – A closing phrase that specifies artistic elements (for example, *“Bold linework, dramatic shadows, flat color palettes”*)

This structured technique enables exploration of various artistic styles, including graphic novels, sketches, and 3D illustrations, while maintaining character consistency throughout the storyboard. The following is an example prompt template and some style information you can experiment with:

```yaml
{style_description} A 7 year old peruvian girl with dark hair in two low braids wearing a
school uniform. {style_details}
styles = [
    {
        "name": "graphic-novel",
        "description": "A graphic novel style illustation of",
        "details": "Bold linework, dramatic shadows, and flat color palettes. Use
            high contrast lighting and cinematic composition typical of comic book
            panels. Include expressive line work to convey emotion and movement.",
    },
    {
        "name": "sketch",
        "description": "A simple black and white line sketch of",
        "details": "Rough, sketch-like lines create a storyboard aesthetic. High
            contrast. No color",
    },
    {
        "name": "digital-illustration",
        "description": "A 3D digital drawing of",
        "details": "High contrast. Rounded character design. Smooth rendering.
            Soft texture. Luminous lighting",
    },
]
```


![Girl](/images/3-BlogsTranslated/3.1-Blog1/image-7-1.jpeg)

### Scene integration with consistent parameters

Now we can put these techniques together to test for character consistency across different narrative contexts, as shown in the following example images. We maintain consistent input for style, `seed`, and `cfgScale`, varying only the scene description to make sure character remains recognizable throughout the scene sequences.

<div style="display: flex; flex-direction: column; gap: 24px; align-items: center; justify-content: center;">
  <div style="display: flex; flex-direction: row; gap: 24px; align-items: flex-start; max-width: 800px;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-8-288x300.jpg" alt="Girl" style="width: 288px; height: 300px;">
    <div>
      A graphic novel style illustration of a 7 year old Peruvian girl with dark hair in two low braids wearing a school uniform <b>riding a bike on a mountain pass</b>. Bold linework, dramatic shadows, and flat color palettes. Use high contrast lighting and cinematic composition typical of comic book panels. Include expressive line work to convey emotion and movement.
    </div>
  </div>
  <div style="display: flex; flex-direction: row; gap: 24px; align-items: flex-start; max-width: 800px;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-9-288x300.jpg" alt="Girl" style="width: 288px; height: 300px;">
    <div>
      A graphic novel style illustration of a 7 year old Peruvian girl with dark hair in two low braids wearing a school uniform <b>walking on a path through tall grass in the Andes</b>. Bold linework, dramatic shadows, and flat color palettes. Use high contrast lighting and cinematic composition typical of comic book panels. Include expressive line work to convey emotion and movement.
    </div>
  </div>
  <div style="display: flex; flex-direction: row; gap: 24px; align-items: flex-start; max-width: 800px;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-10-288x300.jpg" alt="Girl" style="width: 288px; height: 300px;">
    <div>
      A graphic novel style illustration of a 7 year old Peruvian girl with dark hair in two low braids wearing a school uniform <b>eating ice cream at the beach</b>. Bold linework, dramatic shadows, and flat color palettes. Use high contrast lighting and cinematic composition typical of comic book panels. Include expressive line work to convey emotion and movement.
    </div>
  </div>
</div>

### Storyboard development pipeline

Building upon the character consistency techniques we’ve discussed, we can now implement an end-to-end storyboard development pipeline that transforms written scene and character descriptions into visually coherent storyboards. This systematic approach uses our established parameters for style descriptions, `seed` values, and `cfgScale` values to provide character consistency while adapting to different narrative contexts. The following are some example scene and character descriptions:

```yaml
"scenes":[
    {
        "description": "Mayu stands at the edge of a mountainous path, clutching
            a book. Her mother, Maya, kneels beside her, offering words of encouragement
            and handing her the book. Mayu looks nervous but determined as she prepares
            to start her journey."
    },
    {
        "description": "Mayu encounters a 'danger' sign with a drawing of a
            snake. She looks scared, but then remembers her mother's words. She takes a
            deep breath, looks at her book for reassurance, and then searches for a stick
            on the ground."
    },
    {
        "description": "Mayu bravely makes her way through tall grass, swinging
            her stick and making noise to scare off potential snakes. Her face shows a
            mix of fear and courage as she pushes forward on her journey."
    }
],
"characters":{
    "Mayu":  "A 7-year-old Peruvian girl with dark hair in two low braids wearing a
        school uniform",
    "Maya":  "An older Peruvian woman with long dark hair tied back in a bun, wearing
        traditional Peruvian clothing"
}
```
![Diagram](/images/3-BlogsTranslated/3.1-Blog1/image-11-4.png)

Our pipeline uses Amazon Nova Lite to first craft optimized image prompts incorporating our established best practices, which are then passed to Amazon Nova Canvas for image generation. By setting `numberOfImages` higher (typically three variations), while maintaining consistent `seed` and `cfgScale` values, we give creators multiple options that preserve character consistency. We used the following prompt for Amazon Nova Lite to generate optimized image prompts:

```yaml
Describe an image that best represents the scene described. Here are some examples:
scene: Rosa is in the kitchen, rummaging through the pantry, looking for a snack. She
    hears a strange noise coming from the back of the pantry and becomes startled.
imagery: A dimly lit pantry with shelves stocked with various food items, and Rosa
    peering inside, her face expressing curiosity and a hint of fear.
scene: Rosa says goodbye to her mother, Maya. Maya offers her words of encouragement.
imagery: A wide shot of Rosa's determined face, facing Maya and receiving a small wrapped
    gift.
Only describe the imagery. Use no more than 60 words.
scene: {scene_description}
imagery:
```

Our pipeline generated the following storyboard panels.

![Diagram](/images/3-BlogsTranslated/3.1-Blog1/1-1.png)

Mayu stands at the edge of a mountainous path, clutching a book. Her mother, Maya, kneels beside her, offering words of encouragement and handing her the book. Mayu looks nervous but determined as she prepares to start her journey.

![Diagram](/images/3-BlogsTranslated/3.1-Blog1/2-1.png)

Mayu encounters a ‘danger’ sign with a drawing of a snake. She looks scared, but then remembers her mother’s words. She takes a deep breath, looks at her book for reassurance, and then searches for a stick on the ground.

![Diagram](/images/3-BlogsTranslated/3.1-Blog1/3-1.png)
Mayu bravely makes her way through tall grass, swinging her stick and making noise to scare off potential snakes. Her face shows a mix of fear and courage as she pushes forward on her journey.

Although these techniques noticeably improve character consistency, they aren’t perfect. Upon closer inspection, you will notice that even images within the same scene show variations in character consistency. Using consistent `seed` values helps control these variations, and the techniques outlined in this post significantly improve consistency compared to basic prompt engineering. However, if your use case requires near-perfect character consistency, we recommend proceeding to [Part 2](https://aws.amazon.com/blogs/machine-learning/build-character-consistent-storyboards-using-amazon-nova-in-amazon-bedrock-part-2/), where we explore advanced fine-tuning techniques.

### Video generation for animated storyboards

If you want to go beyond static scene images to transform your storyboard into short, animated video clips, you can use Amazon Nova Reel. We use Amazon Nova Lite to convert image prompts into video prompts, adding subtle motion and camera movements optimized for the Amazon Nova Reel model. These prompts, along with the original images, serve as creative constraints for Amazon Nova Reel to generate the final animated sequences. The following is the example prompt and its resulting animated scene in GIF format:

```yaml
A sunlit forest path with a 'Danger' sign featuring a snake. A 7-year-old Peruvian girl
    stands, visibly scared but resolute. Bold linework, dramatic shadows, and flat color
    palettes. High contrast lighting and cinematic composition. Mist slowly drifting.
    Camera dolly in.
```

<div style="display: flex; flex-direction: row; gap: 16px; align-items: flex-start; justify-content: center;">
  <div style="text-align: center;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-15.jpeg" alt="Input image" width="300">
    <div>Input image</div>
  </div>
  <div style="text-align: center;">
    <img src="/images/3-BlogsTranslated/3.1-Blog1/image-16.gif" alt="Output video" width="300">
    <div>Output video</div>
  </div>
</div>

### Conclusion

In this first part of our series, we explored fundamental techniques for achieving character and style consistency using Amazon Nova Canvas, from structured prompt engineering to building an end-to-end storyboarding pipeline. We demonstrated how combining style descriptions, `seed` values, and careful `cfgScale` parameter control can significantly improve character consistency across different scenes. We also showed how integrating Amazon Nova Lite with Amazon Nova Reel can enhance the storyboarding workflow, enabling both optimized prompt generation and animated sequences.
