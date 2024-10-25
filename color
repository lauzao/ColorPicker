import net.minecraft.client.gui.widget.AbstractWidget;
import net.minecraft.client.util.math.DrawContext;
import org.lwjgl.glfw.GLFW;
import java.awt.Color;

public class ColorPickerWidget extends AbstractWidget {
    private int hue;  // A posição da barra de cor (0-360)
    private float saturation;  // Saturação (0.0 - 1.0)
    private float brightness;  // Brilho (0.0 - 1.0)

    // Dimensões do color picker
    private final int gradientWidth = 200;
    private final int gradientHeight = 200;
    private final int barWidth = 20;
    private final int barHeight = gradientHeight;

    public ColorPickerWidget(int x, int y, int width, int height) {
        super(x, y, width, height);
        this.hue = 0;
        this.saturation = 1.0f;
        this.brightness = 1.0f;
    }

    // Método que renderiza o widget
    @Override
    public void render(DrawContext context, int mouseX, int mouseY, float delta) {
        // Renderiza o gradiente de brilho e saturação
        drawColorGradient(context, x, y, gradientWidth, gradientHeight);

        // Renderiza a barra de seleção de cores
        drawColorBar(context, x + gradientWidth + 5, y, barWidth, barHeight);

        // Checa se o mouse está em cima da área de cor
        if (isMouseOverGradient(mouseX, mouseY)) {
            if (GLFW.glfwGetMouseButton(GLFW.glfwGetCurrentContext(), GLFW.GLFW_MOUSE_BUTTON_1) == GLFW.GLFW_PRESS) {
                updateSaturationBrightness(mouseX, mouseY);
            }
        }

        // Checa se o mouse está em cima da barra de cores
        if (isMouseOverColorBar(mouseX, mouseY)) {
            if (GLFW.glfwGetMouseButton(GLFW.glfwGetCurrentContext(), GLFW.GLFW_MOUSE_BUTTON_1) == GLFW.GLFW_PRESS) {
                updateHue(mouseY);
            }
        }

        super.render(context, mouseX, mouseY, delta);
    }

    private void drawColorGradient(DrawContext context, int x, int y, int width, int height) {
        for (int i = 0; i < width; i++) {
            for (int j = 0; j < height; j++) {
                float sat = (float) i / (float) width;
                float bri = 1.0f - (float) j / (float) height;
                int color = Color.HSBtoRGB((float) hue / 360f, sat, bri);
                context.fill(x + i, y + j, x + i + 1, y + j + 1, color);
            }
        }
    }

    private void drawColorBar(DrawContext context, int x, int y, int width, int height) {
        for (int j = 0; j < height; j++) {
            float hueValue = (float) j / (float) height;
            int color = Color.HSBtoRGB(hueValue, 1.0f, 1.0f);
            context.fill(x, y + j, x + width, y + j + 1, color);
        }
    }

    private boolean isMouseOverGradient(int mouseX, int mouseY) {
        return mouseX >= x && mouseX < x + gradientWidth && mouseY >= y && mouseY < y + gradientHeight;
    }

    private boolean isMouseOverColorBar(int mouseX, int mouseY) {
        return mouseX >= x + gradientWidth + 5 && mouseX < x + gradientWidth + 5 + barWidth && mouseY >= y && mouseY < y + barHeight;
    }

    private void updateSaturationBrightness(int mouseX, int mouseY) {
        saturation = (float) (mouseX - x) / (float) gradientWidth;
        brightness = 1.0f - (float) (mouseY - y) / (float) gradientHeight;
    }

    private void updateHue(int mouseY) {
        hue = (int) ((float) (mouseY - y) / (float) barHeight * 360);
    }

    // Método que retorna a cor selecionada
    public int getSelectedColor() {
        return Color.HSBtoRGB((float) hue / 360f, saturation, brightness);
    }
}
